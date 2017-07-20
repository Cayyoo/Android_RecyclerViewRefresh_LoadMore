# RecyclerViewRefresh_LoadMore
SwipeRefreshLayout+RecyclerView实现下拉刷新、上拉加载

```java
/**
 * 使用SwipeRefreshLayout实现下拉刷新
 * 重写RecyclerView的setOnScrollListener实现下拉加载
 *
 * 原始数据未铺满一屏时，即不需要滑动，通过重写RecyclerView的setOnScrollListener无法加载数据
 * 
 * 截图左侧的menu在有左键或右键为功能键的Android手机上可调出来
 */
```

```java
// 下拉刷新
mRefreshLayout.setOnRefreshListener(new OnRefreshListener() {
	@Override
	public void onRefresh() {
		mDatas.clear();
		for (int i = 0; i < 50; i++) {
			mDatas.add("刷新数据---" + new Random().nextInt(30));
		}
		adapter.notifyDataSetChanged();
		mRefreshLayout.setRefreshing(false);
	}
});
    
// 上拉自动加载
mRecyclerView.setOnScrollListener(new OnScrollListener() {
	@Override
	public void onScrolled(RecyclerView recyclerView, int dx, int dy) {
		super.onScrolled(recyclerView, dx, dy);

		LinearLayoutManager manager = (LinearLayoutManager) recyclerView.getLayoutManager();
		int last = manager.findLastCompletelyVisibleItemPosition();
		int totalCount = manager.getItemCount();

		// last >= totalCount - 2表示剩余2个item是自动加载，可自己设置
		// dy>0表示向下滑动
		if (last >= totalCount - 2 && dy > 0) {
			/*
			 * 加载数据
			 */
			List<String> datas = new ArrayList<>();
			for (int i = 0; i < 10; i++) {
				datas.add("加载数据--" + new Random().nextInt(10));
			}
			//addData()是在自定义的Adapter中自己添加的方法，用来给list添加数据
			adapter.addData(datas);
		}
	}
});
```


```java
<RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent" >

    <android.support.v4.widget.SwipeRefreshLayout
        android:id="@+id/swipeRefreshLayout"
        android:layout_width="match_parent"
        android:layout_height="wrap_content" >

        <android.support.v7.widget.RecyclerView
            android:id="@+id/recyclerView"
            android:layout_width="match_parent"
            android:layout_height="match_parent" />

    </android.support.v4.widget.SwipeRefreshLayout>

</RelativeLayout>
```

![](https://github.com/ykmeory/RecyclerViewRefresh_LoadMore/blob/master/Screenshot_2017-07-21-01-45-49.jpg "截图")
