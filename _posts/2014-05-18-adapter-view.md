---
layout: post
title:  "Android的Adapter和AdapterView"
date:   2014-05-18 14:06:05
categories: android
---

* content
{:toc}

Adapter类图

<img src="/images/adapter.png" width = "800" alt="Android Adapter"/>

AdapterView类图

<img src="/images/adapterview.png" width = "800" alt="Android AdapterView"/>

常见的ListView使用ListAdapter

    public void setAdapter(ListAdapter adapter)

因此使用BaseAdapter，ArrayAdapter，SimpleAdapter等都是可以的

与此类似，

GridView使用ListAdapter

Spinner和Gallery都继承自AbsSpinner，都使用SpinnerAdapter
