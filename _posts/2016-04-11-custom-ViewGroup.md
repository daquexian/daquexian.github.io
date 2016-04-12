---
layout: post

title: 自定义滑动时吸附屏幕的ViewGroup

date: 2016-04-11

categories: blog

tags: [Android, 自定义View]

description: 

---

整个逻辑并不复杂，重写onMeasure方法及onLayout方法实现每个子View占据一屏幕的大小，重写onTouchEvent方法实现滑动和“吸附”。  
代码如下：

	package com.example.jianhao.testlistview;

	import android.content.Context;
	import android.util.AttributeSet;
	import android.view.MotionEvent;
	import android.view.View;
	import android.view.ViewGroup;
	import android.widget.ImageView;
	import android.widget.Scroller;

	/**
	 * Created by daquexian on 16-4-11.
	 * custom ViewGroup to which child view "stick" when scrolling
	 */
	public class MyViewGroup extends ViewGroup {
	    private int mLastY, mStart, mEnd;
	    private int mParentHeight;
	    private Scroller mScroller = new Scroller(getContext());    //Scroll object helps scroll smoothly

	    @Override
	    public ViewGroup.LayoutParams generateLayoutParams(AttributeSet attrs)
	    {
		return new MarginLayoutParams(getContext(), attrs);
	    }

	    //must implement all three constructors
	    public MyViewGroup(Context context){
		super(context);
	    }

	    public MyViewGroup(Context context, AttributeSet attrs){
		super(context, attrs);
	    }

	    public MyViewGroup(Context context, AttributeSet attrs, int defStyle){
		super(context, attrs, defStyle);
	    }

	    @Override
	    protected void onMeasure(int widthMeasureSpec, int heightMeasureSpec) {
		super.onMeasure(widthMeasureSpec, heightMeasureSpec);
		int count = getChildCount();
		for(int i = 0; i < count; i++){
		    View childView = getChildAt(i);
		    measureChild(childView, widthMeasureSpec, heightMeasureSpec);
		}
		mParentHeight = ((View)getParent()).getHeight();
		//set height in onMeasure()
		setMeasuredDimension(MeasureSpec.getSize(widthMeasureSpec), count * mParentHeight);
	    }

	    @Override
	    protected void onLayout(boolean changed, int l, int t, int r, int b) {
		int childCount = getChildCount();
		for(int i = 0; i < childCount; i++){
		    View childView = getChildAt(i);
		    childView.layout(l, i * mParentHeight, r, (i + 1) * mParentHeight);
		    //stretch image to fit in with ImageView
		    ((ImageView)childView).setScaleType(ImageView.ScaleType.FIT_XY);
		}
	    }

	    @Override
	    public boolean onTouchEvent(MotionEvent event) {
		int y = (int) event.getY();
		switch (event.getAction()){
		    case MotionEvent.ACTION_DOWN:
		        mLastY = y;
		        mStart = getScrollY();
		        break;
		    case MotionEvent.ACTION_MOVE:
		        if(!mScroller.isFinished()){
		            mScroller.abortAnimation();
		        }

		        int dy = mLastY - y;

		        if(getScrollY() < 0 || getScrollY() > getHeight() - mParentHeight){
		            dy = 0;
		        }

		        scrollBy(0, dy);
		        mLastY = y;
		        break;
		    case MotionEvent.ACTION_UP:
		        mEnd = getScrollY();

		        int dScrollY = mEnd - mStart;
		        if(dScrollY > 0){
		            if(dScrollY < mParentHeight / 3){
		                mScroller.startScroll(0, getScrollY(), 0, -dScrollY);
		            }else{
		                mScroller.startScroll(0, getScrollY(), 0, mParentHeight - dScrollY);
		            }
		        }else{
		            if(-dScrollY < mParentHeight / 3){
		                mScroller.startScroll(0, getScrollY(), 0, -dScrollY);
		            }else{
		                mScroller.startScroll(0, getScrollY(), 0, -mParentHeight - dScrollY);
		            }
		        }

		        /* Or code following:
		        if(Math.abs(dScrollY) < mParentHeight / 3){
		            mScroller.startScroll(0, getScrollY(), 0, -dScrollY);
		        }else{
		            mScroller.startScroll(0, getScrollY(), 0, (Math.abs(dScrollY) / dScrollY) * mParentHeight - dScrollY);
		        }
		        */
		        break;
		}
		//force refresh view
		postInvalidate();
		return true;
	    }

	    //called by system when view is to refresh
	    @Override
	    public void computeScroll() {
		super.computeScroll();
		//when scrolling of Scroller is not finished, Scroller.computeScrollOffset() return true
		if(mScroller.computeScrollOffset()){
		    scrollTo(0, mScroller.getCurrY());
		}
		postInvalidate();
	    }
	}

