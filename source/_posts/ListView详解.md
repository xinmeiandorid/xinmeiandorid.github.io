---
title: Listview详解    
---

![][1]

## 1 ListView的官⽅描述

>A view that shows items in a vertically scrolling list. The items come from the ListAdapter associated with this view.

所以本⽂从以下三个⻆度来学习⼀下ListView，同时还会涉及到ListView的复⽤机制RecycleBin。

- A view that shows items
- vertically scrolling
- The items come from the ListAdapter

## 2 A view that shows items

从本质上来看ListView就是⼀个ViewGroup ,它也是由⼀个⼀个的ChildView组成的，每⼀个ItemView就是ListView的ChildView。

那么ListView是怎么显示这些ItemView的呢？和普通View⼀样，它也需要经过Measure， Layout， Draw三个环节。

### 2.1 Measure

1. 以 AT_MOST 的⽅式测量时

    ListView会依次测量ItemView的⾼度并累加，如果超过了最⼤⾼度了就以最⼤⾼度作为ListView的⾼度；如果所有ItemView都累加起来还不到最⼤⾼度，则以所有ItemView的累加值为ListView的⾼度。
2. 以 UNSPECIFIED 的⽅式测量时

    ListView会以第⼀个ItemView的宽⾼来决定⾃⼰的宽⾼。
3. 以 EXACTLY 的⽅式测量时 

    ListView不做特殊处理。通常情况下ListView都会⽤MATCH_PARENT ，就是⽤这种⽅式测量的。

ListView的Measure有⼀个⽐较特殊的地⽅，它不会去measure child， child的measure会在ListView的Layout过程中同时完成。

### 2.2 Draw

作为⼀个ViewGroup， ListView除了绘制⼀些divider和Overscroll效果等附加元素外， ItemView的绘制都派发给ItemView⾃⼰去完成。

### 2.3 Layout

Layout环节是ListView把ItemView摆放在ListView的过程，这是ListView最关键的环节。但是在对ItemView进⾏布局之前，我们需要先获得ItemView；获取⼀个ItemView会涉及到ItemView的创建和复⽤。所以在学习深⼊Layout的过程前，我们先学习⼀下ItemView的创建（ListAdapter）和ItemView的复⽤（RecycleBin）。

1. ItemView的创建(ListAdapter)

    通过继承关系我们可以看到ListView是⼀个AdapterView<ListAdapter> ，它是通过ListAdapter的来创建ItemView的。ListAdapter作为ListView和数据源之间的中间⼈，负责把数据源中的数据项转化成ItemView供ListView使⽤。

    ListView不再关⼼具体的数据源，也不关⼼ItemView⻓什么样⼦；它只需要告诉ListAdapter，我要第⼏个数据项的ItemView， ListAdapter负责根据对应数据项创建出ItemView返回给ListView。 ListView只需要负责把ItemView作为ChildView添加到⾃⼰的布局上。

    ListAdapter的主要⽅法如下：

    ```lua
    //返回数据源的数量
    int getCount();
    //返回指定position的ItemView
    View getView(int position,View convertView,ViewGroup parent);
    ```
2. getView方法
  getView⽅法除了负责创建ItemView外还负责把数据和ItemView绑定，也就是把对应位置的数据项内容填充到ItemView上。

    第⼆个参数 View convertView表示可以复⽤的ItemView，如果convertView为空，getView⽅法需要新建⼀个ItemView；如果convertView不为空，getView不需要再新建ItemView了，直接把对应数据项和convertView进⾏绑定就可以了。

    需要注意的是，⼀般情况下当我们提到position的时候都是指数据源中的position，区别于ItemView在ListView作为ChildView的Index，下⽂没有专⻔指出时也是如此。

    虽然ListView不关⼼ItemView⻓什么样⼦，但是ListView在复⽤机制中需要知道ItemView有多少种展现形式，只有相同的展现形式的ItemView才可以被复⽤。所以以下两个ViewType相关的⽅法也⽐较重要。

    ```lua
    //返回⼀共有多少种ViewType的ItemView
    int getViewTypeCount();
    //返回指定position的ItemView是哪⼀种ViewType
    int getItemViewType(int	position);
    ```
3. ItemView的复用(RecycleBin)
  ListView除了通过ListAdapter获取ItemView，还可以通过RecycleBin对已经创建的ItemView进⾏回收和复⽤。有了复⽤机制，ListView不需要每次都新建ItemView，可以⼤幅提⾼ListView的显示和滚动效率。

RecycleBin通过两级缓存来回收ItemView

```lua
View[]	mActiveViews
ArrayList<View>[]	scrapViews	= new ArrayList[viewTypeCount]
```

如果数据没有发⽣变化， ListView刚开始Layout时会把当前界⾯上的ItemView存放到mActiveViews缓存，这些ItemView是连续位置的，通过 int mFirstActivePosition 来标定第⼀个ActiveView在数据源中的位置。

在同⼀轮Layout中相同位置的ItemView可以通过 View getActiveView(int position) 直接使⽤原来的ActiveView：它只需要简单的attach到ListView上，不需要再通过ListAdapter来数据进⾏绑定。⽐如ListView上的某个ItemView调⽤requestLayout导致界⾯刷新时， ListView可以利⽤ActiveView很快地再次完成布局。

getActiveView⽅法源码如下：
```lua
View getActiveView(int	position) {
    int	index =	position - mFirstActivePosition;
    final View[] activeViews = mActiveViews;
    if (index >=0 && index < activeViews.length) {
    final View match = activeViews[index];
	activeViews[index] = null;
    return match;
    }
    return null;
}
```

如果Layout时发现数据发⽣了变化，那么我们相同位置也不能再直接使⽤原来的ItemView了，所以RecycleBin会把ItemView回收到scrapViews中。通过创建语句 ArrayList<View>[] scrapViews = new ArrayList[viewTypeCount];

我们可以知道scrapViews是⼀个ArrayList的数组，他通过ViewType来分类，同⼀个ViewType的ItemView缓存在同⼀个ArrayList中。

当ListView通过 View getScrapView(int position) 来获取缓存的ItemView时，它⾸先会通过mAdapter.getItemViewType(position)向ListAdapter查询正确的ViewType，也就是scrapViews的数组下标，从⽽获得正确类型的ItemView。因为数据已经发⽣了变化，通过scrapViews获取的ItemView只能保证类型相同，但是显示内容已经发⽣变化，所以需要通过getView⽅法进⾏再次绑定，通过getScrapView获得的ItemView就是传给getView⽅法的convertView。

getScrapView⽅法源码如下：

```lua
View getScrapView(int position) {
    final int whichScrap = mAdapter.getItemViewType(position);
    if (whichScrap < 0) {
        return	null;
    }
    if (mViewTypeCount	== 1) {
    return retrieveFromScrap(mCurrentScrap,	position);
    } else if (whichScrap < mScrapViews.length) {
    return retrieveFromScrap(mScrapViews[whichScrap],position);
    }
    return null;
}
```
除此之外， RecycleBin还有⼀个 ArrayList<View> mSkippedScrap ，⾥⾯暂存⽆法复⽤的View⽐如HeaderView、FooterView等。

## 3 Layout
了解完ListAdapter和RecycleBin的基本原理后，我们再继续结合onLayout⽅法的源码来学习ListView最重要的Layout过程。
```lua
protected void onLayout(boolean	changed, int l,int t,int r,int b) {
    super.onLayout(changed,l,t,r,b);
	mInLayout = true;
    final int childCount = getChildCount();
    if (changed) {
    for (int	i	= 0;	i	<	childCount;	i++) {
    getChildAt(i).forceLayout();
    }
	mRecycler.markChildrenDirty();
} .
..
    layoutChildren();
	mInLayout = false;
...
}
```
onLayout⽅法⾸先通过changed参数判断size有没有变化，如果有变化，那么调⽤ListView当前显示的所有ChildView的forceLayout⽅法， mRecycler.markChildrenDirty()对RecycleBin中缓存的所有ItemView调⽤forceLayout，确保它们再次使⽤的时候会被重新布局和绘制。接着，onLayout调⽤layoutChildren把ItemView⼀个⼀个添加到ListView上去。

```lua
protected void layoutChildren() {
    final boolean blockLayoutRequests = mBlockLayoutRequests;
    if (!blockLayoutRequests) {
        mBlockLayoutRequests = true;
    }
    try {
        super.layoutChildren();
        /****************************代码块1****************************/
        invalidate();
        /****************************代码块1****************************/
        ...
        final int childrenTop = mListPadding.top;
        final int childrenBottom = mBottom - mTop - mListPadding.bottom;
        int childCount = getChildCount();
        int index;
        int delta = 0;
        View sel;
        /****************************代码块2****************************/
        //	Handle	the	empty	set	by	removing	all	views	that	are	visible
        //	and	calling	it	a	day
        if (mItemCount == 0) {
            resetList();
            invokeOnItemScrollListener();
            return;
        } else if (mItemCount != mAdapter.getCount()) {
            throw new IllegalStateException("The content of	the	adapter	has	changed	but	"
                    + "ListView	did	not	receive	a notification.	Make sure the content of "
                    + "your	adapter	is	not	modified from a	background	thread,	but	only from "
                    + "the UI thread. Make sure	your adapter calls notifyDataSetChanged() "
                    + "when	its	content	changes. [in ListView(" + getId() + ",	" + getClass()
                    + ") with Adapter(" + mAdapter.getClass() + ")]");
        }
        /****************************代码块2****************************/
        ...
        //	Pull	all	children	into	the	RecycleBin.
        //	These	views	will	be	reused	if	possible
        final int firstPosition = mFirstPosition;
        final RecycleBin recycleBin = mRecycler;
        /****************************代码块3****************************/
        if (dataChanged) {
            for (int i = 0; i < childCount; i++) {
                recycleBin.addScrapView(getChildAt(i), firstPosition + i);
            }
        } else {
            recycleBin.fillActiveViews(childCount, firstPosition);
        }
        //	Clear	out	old	views
        detachAllViewsFromParent();
        recycleBin.removeSkippedScrap();
        /****************************代码块3****************************/
        /****************************代码块4****************************/
        switch (mLayoutMode) {
            case LAYOUT_SET_SELECTION:
                if (newSel != null) {
                    sel = fillFromSelection(newSel.getTop(), childrenTop, childrenBottom);
                } else {
                    sel = fillSelection(childrenTop, childrenBottom);
                }
                break;
            case LAYOUT_FORCE_TOP:
                mFirstPosition = 0;
                sel = fillFromTop(childrenTop);
                adjustViewsUpOrDown();
                break;
            case LAYOUT_FORCE_BOTTOM:
                sel = fillUp(mItemCount - 1, childrenBottom);
                adjustViewsUpOrDown();
                break;
            case LAYOUT_SPECIFIC:
                sel = fillSpecific(mSelectedPosition, mSpecificTop);
                break;
            case LAYOUT_SYNC:
                sel = fillSpecific(mSyncPosition, mSpecificTop);
                break;
            case LAYOUT_MOVE_SELECTION:
                //	Move	the	selection	relative	to	its	old	position
                sel = moveSelection(delta, childrenTop, childrenBottom);
                break;
            default:
                if (childCount == 0) {
                    if (!mStackFromBottom) {
                        setSelectedPositionInt(mAdapter == null || isInTouchMode() ?
                                INVALID_POSITION : 0);
                        sel = fillFromTop(childrenTop);
                    } else {
                        final int last = mItemCount - 1;
                        setSelectedPositionInt(mAdapter == null || isInTouchMode() ?
                                INVALID_POSITION : last);
                        sel = fillFromBottom(last, childrenBottom);
                    }
                } else {
                    if (mSelectedPosition >= 0 && mSelectedPosition < mItemCount) {
                        sel = fillSpecific(mSelectedPosition, oldSel == null ?
                                childrenTop : oldSel.getTop());
                    } else if (mFirstPosition < mItemCount) {
                        sel = fillSpecific(mFirstPosition, oldFirst == null ?
                                childrenTop : oldFirst.getTop());
                    } else {
                        sel = fillSpecific(0, childrenTop);
                    }
                }
                break;
        }
        /****************************代码块4****************************/
        /****************************代码块5****************************/
        //	Flush	any	cached	views	that	did	not	get	reused	above
        recycleBin.scrapActiveViews();
        /****************************代码块5****************************/
        ...
    } finally {
        if (!blockLayoutRequests) {
            mBlockLayoutRequests = false;
        }
    }
}
```
上⾯是精简过的layoutChildren的源码，主要省略了Selection和Accessibility相关的代码，我们主要关注注释标注出来的代码块。

- 代码块1 调⽤了invalidate确保之后加⼊到ListView的ItemView以及ListView的Divider等附加元素可以确保被重绘。
- 代码块2 如果mItemCount不为0，为了确保数据⼀致性， ListView会把它和 mAdapter.getCount() 进⾏⽐较，⼀旦出现不相等的情况就会抛出ListView中很典型的IllegalStateException。当我们在⼦线程中处理数据或者修改了数据后没有调⽤notifyDataSetChanged 时很容易出现这种情况。
- 代码块3 在开始Layout之前，ListView会把当前显示的ItemView回收起来：如果数据发⽣了变化，⽐如我们调⽤了notifyDataSetChanged后， ListView把它们回收到ScrapViews中；如果数据没有发⽣变化， ListView把它们回收到ActiveViews中。接着，⽆论回收到哪个缓存，所有View都要从parent也就是ListView中detach。最后RecycleBin会把那些⽆法复⽤的SkipScrapView释放掉。
- 代码块4 根据不同的布局⽅式mLayoutMode对ListView进⾏布局， ListView有以下⼏种布局⽅式：
    - LAYOUT_NORMAL 常规布局⽅式
    - LAYOUT_FORCE_TOP ListView滑动到顶部，显示第⼀个Item的⽅式
    - LAYOUT_SET_SELECTION 指定ItemView显示在指定位置的布局⽅式
    - LAYOUT_FORCE_BOTTOM ListView滑动到底部，显示最后⼀个Item的布局⽅式
    - LAYOUT_SPECIFIC 指定ItemView显示在顶部的布局⽅式
    - LAYOUT_SYNC 由于数据变化重新布局时，把原来第⼀个ItemView恢复到相同位置的布局
    - LAYOUT_MOVE_SELECTION ⽤实体导航键对ListView进⾏滚动时的布局⽅式

我们看⼀下常规布局⽅式，也就是default分⽀。

default分⽀⼜分为两种情况：
第⼀种 childCount == 0 的情况，⽐如ListView第⼀次显示时。如果ListView没有设置stackFromBottom属性，那么ListView会通过 fillFromTop(childrenTop) 从上到下添加ItemView。
```lua
private View fillFromTop(int nextTop) {
    mFirstPosition = Math.min(mFirstPosition, mSelectedPosition);
    mFirstPosition = Math.min(mFirstPosition, mItemCount - 1);
    if (mFirstPosition < 0) {
        mFirstPosition = 0;
    }
    return fillDown(mFirstPosition, nextTop);
}
```

fillFromTop主要通过fillDown来完成，传⼊的参数 mFirstPosition 和 nextTop 代表第⼀个ItemView的position和顶点位置。

ListView第⼀次显示时，这两个值分别是0和ListView的paddingTop
```lua
private View fillDown(int pos, int nextTop) {
    View selectedView = null;
    int end = (mBottom - mTop);
    if ((mGroupFlags & CLIP_TO_PADDING_MASK) == CLIP_TO_PADDING_MASK) {
        end -= mListPadding.bottom;
    }
    while (nextTop < end && pos < mItemCount) {
        //	is	this	the	selected	item?
        boolean selected = pos == mSelectedPosition;
        View child = makeAndAddView(pos, nextTop, true, mListPadding.left, selected);
        nextTop = child.getBottom() + mDividerHeight;
        if (selected) {
            selectedView = child;
        }
        pos++;
    }
    setVisibleRangeHint(mFirstPosition, mFirstPosition + getChildCount() - 1);
    return selectedView;
}
```
fillDown中先计算出代表ItemView总⾼度的end值，然后不断地通过makeAndAddView创建出ItemView并添加到ListView。随着ItemView的增多， nextTop也不断地增加。

⼀旦nextTop >= end了，也就意味着这个ItemView肯定显示在ListView之外了，也就是说我们的整个ListView已经被填充满了，所以结束while循环；或者 pos >= mItemCount了，意味着数据源中的所有Item项的ItemView都已经显示出来了，也要结束while循环。

这其中最重要的就是 makeAndAddView ⽅法。

```lua
private View makeAndAddView(int position, int y, boolean flow, int childrenLeft,
                            boolean selected) {
    View child;
    if (!mDataChanged) {
        //	Try	to	use	an	existing	view	for	this	position
        child = mRecycler.getActiveView(position);
        if (child != null) {
            //	Found	it	--	we're	using	an	existing	child
            //	This	just	needs	to	be	positioned
            setupChild(child, position, y, flow, childrenLeft, selected, true);
            return child;
        }
    }
    //	Make	a	new	view	for	this	position,	or	convert	an	unused	view	if	possible
    child = obtainView(position, mIsScrap);
    //	This	needs	to	be	positioned	and	measured
    setupChild(child, position, y, flow, childrenLeft, selected, mIsScrap[0]);
    return child;
}
```

如果数据没有改变， ListView直接从RecycleBin中的ActiveView缓存中取对应位置的ItemView。如果可以在ActiveView缓存中取到，那么通过setupChild把它添加到ListView。如果没有取到，继续通过obtainView来获取。
```lua
View obtainView(int position, boolean[] isScrap) {
    Trace.traceBegin(Trace.TRACE_TAG_VIEW, "obtainView");
    isScrap[0] = false;
     ...
    final View scrapView = mRecycler.getScrapView(position);
    final View child = mAdapter.getView(position, scrapView, this);
    if (scrapView != null) {
        if (child != scrapView) {
            //	Failed	to	re-bind	the	data,	return	scrap	to	the	heap.
            mRecycler.addScrapView(scrapView, position);
        } else {
            isScrap[0] = true;
            //	Finish	the	temporary	detach	started	in	addScrapView().
            child.dispatchFinishTemporaryDetach();
        }
    }
    ...
    Trace.traceEnd(Trace.TRACE_TAG_VIEW);
    return child;
}
```
obtainView⾸先从ScrapView缓存取ItemView，⽆论有没有取到，都会调⽤上⽂提到的ListAdapter的 View getView(int position, View convertView, ViewGroup parent) ⽅法。

如果这⾥取到缓存的ItemView，也就是scrapView不为空，那么getView可以直接使⽤convertView作为ItemView；但是这个ItemView只是ViewType和ListView需要的ItemView⼀样，但是内容已经不⼀样了，所以getView还是需要重新绑定数据；

如果从ScrapView缓存没有取到ItemView，也就是scrapView为空，也就是convertView为空，所以getView需要新建⼀个ItemView并绑定数据。

在 scrapView != null 的前提下如果我们发现 child != scrapView ，也就是getView并没有使⽤我们传⼊的scrapView（这是我们getView⽅法的写法有问题导致的，需要避免），那么ListView继续把这个没有使⽤的scrapView再次放到ScrapView缓存。

如果我们使⽤了缓存的scrapView，我们把isScrap[0]置为true，表示ItemView是从ScrapView缓存获取的。通过obtainView获得ItemView后也是调⽤setupChild⽅法。通过上⾯的分析，我们可以发现makeAndAddView中把RecycleBin和ListAdapter都结合起来了。

```lua
private void setupChild(View child, int position, int y, boolean flowDown, int childrenLeft,
                        boolean selected, boolean recycled) {
    ...
    final boolean needToMeasure = !recycled || updateChildSelected || child.isLayoutRequested();
    ...
    if ((recycled && !p.forceAdd) || (p.recycledHeaderFooter
            && p.viewType == AdapterView.ITEM_VIEW_TYPE_HEADER_OR_FOOTER)) {
        attachViewToParent(child, flowDown ? -1 : 0, p);
    } else {
        p.forceAdd = false;
        if (p.viewType == AdapterView.ITEM_VIEW_TYPE_HEADER_OR_FOOTER) {
            p.recycledHeaderFooter = true;
        }
        addViewInLayout(child, flowDown ? -1 : 0, p, true);
    } .
    if (needToMeasure) {
        final int childWidthSpec = ViewGroup.getChildMeasureSpec(mWidthMeasureSpec,
                mListPadding.left + mListPadding.right, p.width);
        final int lpHeight = p.height;
        final int childHeightSpec;
        if (lpHeight > 0) {
            childHeightSpec = MeasureSpec.makeMeasureSpec(lpHeight, MeasureSpec.EXACTLY);
        } else {
            childHeightSpec = MeasureSpec.makeSafeMeasureSpec(getMeasuredHeight(),
                    MeasureSpec.UNSPECIFIED);
        }
        child.measure(childWidthSpec, childHeightSpec);
    } else {
        cleanupLayoutState(child);
    }
    final int w = child.getMeasuredWidth();
    final int h = child.getMeasuredHeight();
    final int childTop = flowDown ? y : y - h;
    if (needToMeasure) {
        final int childRight = childrenLeft + w;
        final int childBottom = childTop + h;
        child.layout(childrenLeft, childTop, childRight, childBottom);
    } else {
        child.offsetLeftAndRight(childrenLeft - child.getLeft());
        child.offsetTopAndBottom(childTop - child.getTop());
    } .
}
```

最后我们分析⼀下setupChild⽅法。

注意看它的最后⼀个参数 boolean recycled ，makeAndAddView中传⼊的是true和isScrap[0]，因为⽆论是从ActiveView缓存还是ScrapView缓存获取，都表示这个ItemView是重复利⽤的。如果传⼊recycled为false，那么needToMeasure会被置为true。

因为不是从缓存获取的ItemView肯定是新建的，它还没有被Measure过。除此之外，如果这个ItemView的Select状态改变了，或者⾃⼰发起了RequestLayout， needToMeasure也会被置为true。

通常情况下，从ScrapView缓存获取的ItemView经过getView绑定后，⾥⾯控件内容会修改，isLayoutRequested()会为true，所以也是需要重新measure的。只有通过ActiveView缓存获取才不需要重新measure。接着就要把ItemView加⼊ListView了。如果符合 recycled && !p.forceAdd ，通过 attachViewToParent 把ItemView加⼊ListView，否则通过 addViewInLayout 加⼊。 

attachViewToParent 是⼀个⽐ addViewInLayout 更轻量级的操作，只适合于曾经attach过但是⼜被detach的ChildView使⽤。之所以前⾯的判断条件中还要加⼀个 !p.forceAdd 是因为有⼀些ItemView是刚开始在ListView的onMeasure⽅法中为了Measure⽽创建出来的， Measure完了以后它们没有被销毁，也放⼊了ScrapView缓存⽤于复⽤；也就是说这些ItemView并没有被attach到ListView过，所以它们还是需要调
⽤ addViewInLayout 。

最后，就是根据needToMeasure标志对ChildView进⾏measure和layout。那些不需要重新的measure的ChildView只需要把偏移的位置进⾏校正就可以了。childCount != 0 以及其他case下ListView会通过fillSpecific⽅法以及其他⼀些fillXXX的⽅法来填充数据，除了⽅向上有⼀些变化，⼤致流程是⼀样的，这⾥不再做详细分析。

代码块5 把没有被复⽤的ActiveView转到ScrapView缓存中。⽐如由于某个ItemView⾼度变⼤引发的Layout过程，最后显示在屏幕上的ItemView数量可能会⼩于原来的，那么就会有多余的ActiveView不能被复⽤。

⾄此， ListView通过onLayout⽅法把ItemView进⾏布局的流程就分析完了。 ListView还有⼀种重要的使⽤场景：⽤户通过触摸滑动ListView，在滑动过程中， ListView会依次把数据源中的数据项⼀⼀显示出来。下⾯我们就来分析⼀下随着滑动， ListView不断变化显示出来的ItemView的过程。

## 4 vertically scrolling

ListView通过onTouchEvent⽅法来处理触摸事件，触摸事件的具体解析流程不再详细解释，我们看⼀下下⾯触摸事件引发

1. ListView滚动的主流程

- ACTIONDOWN->onTouchDown->TOUCHMODE_DOWN
- ACTIONMOVE->onTouchMove->startScrollIfNeeded->TOUCHMODE_SCROLL
- ACTION_MOVE->onTouchMove->scrollIfNeeded->trackMotionScroll

onTouchEvent⾸先收到ACTIONDOWN事件，在onTouchDown⽅法中把TouchMode标为TOUCHMODE_DOWN；

接着onTouchEvent收到ACTIONMOVE，在onTouchMove⽅法中调⽤scrollIfNeeded⽅法，如果Move距离⾜够⼤， TouchMode被改为TOUCHMODE_SCROLL；

进⼊TOUCHMODESCROLL状态后，再收到ACTION_MOVE事件，onTouchMove就开始执⾏scrollIfNeeded⽅法了。因为ListView只⽀持垂直⽅向滑动，所以scrollIfNeeded⽅法通过incrementalDeltaY检测到垂直⽅向有移动就进⼊trackMotionScroll开始对ListView进⾏scroll。
```lua
if(incrementalDeltaY ! =0){
    atEdge = trackMotionScroll(deltaY, incrementalDeltaY);
}
```

```lua
boolean trackMotionScroll(int deltaY, int incrementalDeltaY) {
    vertically scrolling...
/****************************代码块1****************************/
    final boolean down = incrementalDeltaY < 0;
/****************************代码块2****************************/
...
/****************************代码块2****************************/
    if (down) {
        int top = -incrementalDeltaY;
        if ((mGroupFlags & CLIP_TO_PADDING_MASK) == CLIP_TO_PADDING_MASK) {
            top += listPadding.top;
        }
        for (int i = 0; i < childCount; i++) {
            final View child = getChildAt(i);
            if (child.getBottom() >= top) {
                break;
            } else {
                count++;
                int position = firstPosition + i;
                if (position >= headerViewsCount && position < footerViewsStart) {
//	The	view	will	be	rebound	to	new	data,	clear	any
//	system-managed	transient	state.
                    child.clearAccessibilityFocus();
                    mRecycler.addScrapView(child, position);
                }
            }
        }
/****************************代码块2****************************/
    } else {
        int bottom = getHeight() - incrementalDeltaY;
        if ((mGroupFlags & CLIP_TO_PADDING_MASK) == CLIP_TO_PADDING_MASK) {
            bottom -= listPadding.bottom;
        }
        for (int i = childCount - 1; i >= 0; i--) {
            final View child = getChildAt(i);
            if (child.getTop() <= bottom) {
                break;
            } else {
                start = i;
                count++;
                int position = firstPosition + i;
                if (position >= headerViewsCount && position < footerViewsStart) {
//	The	view	will	be	rebound	to	new	data,	clear	any
//	system-managed	transient	state.
                    child.clearAccessibilityFocus();
                    mRecycler.addScrapView(child, position);
                }
            }
        }
    } ...
    /****************************代码块3****************************/
    if (count > 0) {
        detachViewsFromParent(start, count);
        mRecycler.removeSkippedScrap();
    }
/****************************代码块3****************************/
...
/****************************代码块4****************************/
    offsetChildrenTopAndBottom(incrementalDeltaY);
/****************************代码块4****************************/
    ...
/****************************代码块5****************************/
    final int absIncrementalDeltaY = Math.abs(incrementalDeltaY);
    if (spaceAbove < absIncrementalDeltaY || spaceBelow < absIncrementalDeltaY) {
        fillGap(down);
    }
/****************************代码块5****************************/
    ...
    return false;
}
```

现在我们分析⼀下trackMotionScroll⽅法：

- 代码块1 根据incrementalDeltaY决定是否为down（这⾥似乎和直觉相反，incrementalDeltaY < 0， ListView应该是向上滑才对；不过Android的Scroll体系的⽅向⼀直和直觉相反，可能它的⽅向是从被Scroll的View的画⾯背景来考虑的，不是从View本身的Scroll⽅向 参考链接1， 参考链接2）。
- 代码块2 如果我们⼿指向上滑动， incrementalDeltaY < 0 ，那么我们进⼊down的处理流程。它从上到下遍历
  ItemView，并通过 child.getBottom() >= top 来判断ItemView是不是还在ListView显示范围内。如果child.getBottom() >= top 不成⽴，说明这个child已经完全在ListView上⽅了，所以把它回收到ScrapView；如
  果成⽴意味着⾄少child可以部分显示，所以从这后⾯的child都不需要再回收了，结束for循环。
- 代码块3 把上⾯回收的ItemView从ListView移除
- 代码块4 把ListView剩余的ItemView的位置进⾏移动，也就是ListView进⾏了滑动
- 代码块5 上⾯的步骤剩余的ItemView项进⾏移动后就会有空⽩区域空出来，所以需要fillGap⽅法把这些空⽩处⽤新的ItemView填充满。
  fillGap⽅法和前⾯onLayout中的fillXXX⽅法类似，也是通过先RecycleBin后ListAdapter两种⽅法获取ItemView。所以ListView在滑动过程中展现出⼤量样式不同的ItemView时也不会出现OOM，因为在RecycleBin的帮助下ListView其实只创建了有限个数的ItemView。
## 5 小节
从 ListView如何显示Item 、 ListView如何滑动 、 RecycleBin如何缓存ItemView 、 ListAdapter如何创建ItemView四个⻆度分析了ListView.

[1]: http://static.zybuluo.com/darren6/o184n24631wsi2z27hlv66ui/image.png
