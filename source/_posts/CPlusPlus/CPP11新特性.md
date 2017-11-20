---
title: C++ 11 中的新特性
date: 2016-12-26 18:38:47
description: C++ 11 新加入的特性
tags: C++
toc: true
---

C++ 11 新特性学习笔记：
`default` 和 `delete` 修饰特殊成员函数。
<!-- More -->

## Defaulted and Deleted function

## Asynchronous Programming

{% alert warning %}
以下三点都满足时， std::future 析构可能导致阻塞：
{% endalert %}
{% textcolor warning %}
1. 共享状态是在调用 std::async 时创建的；<br>
2. 共享状态尚未处于 std::future_status::read 状态；<br>
3. 被析构的当前对象持有共享状态的最后一个引用。<br>
{% endtextcolor %}

{% alert info %}
获取 std::future 结果的方法：
{% endalert %}
{% textcolor info %}
1. std::future::get()&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;阻塞当前线程，等待异步操作完成并返回异步执行结果。<br>
2. std::future::wait()&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;阻塞当前线程，等待异步操作完成，无返回值。<br>
3. std::future::wait_for()&nbsp;&nbsp;&nbsp;&nbsp;阻塞当前线程，超时等待异步操作完成，返回异步操作执行状态。<br>
4. std::future::wait_until()&nbsp;&nbsp;阻塞当前线程，等待异步操作完成直到指定的时间点。<br>
{% endtextcolor %}

|Name|Description|
|:--|:---|
| future::get | Retrieves the result that is stored in the associated asynchronous state. |
| future::wait | Blocks the current thread until the associated asynchronous state is ready. |
| future::wait_for | Blocks until the associated asynchronous state is ready <br>or until the specified time has elapsed. |
| future::wait_until | Blocks until the associated asynchronous state is ready or until a specified point in time. |
<br>

{% alert info %}
与 std::future 相关的枚举类型：
{% endalert %}

* std::future_errc

| label | value | description |
| :---- | :---: | :---------- |
| broken_promise | 0 | The promise object with which the future shares its shared state was destroyed before being set a value or an exception. |
| future_already_retrieved | 1 | A future object was already retrieved from this provider. |
| promise_already_satisfied |2 | The promise object was already set a value or exception. |
| no_state | 3 | An operation attempted to access the shared state of an object with no shared state. |

* std::future_status

| label | value | description |
| :---- | :---: | :---------- |
| future_status::ready | 0 | The function returned because the shared state was ready. |
| future_status::timeout | 1 | The function returned because the specified time was exhausted. |
| future_status::deferred | 2 | The function returned because the shared state contains a deferred function (see std::async). |


* std::launch

| label | value | description |
| :---- | :---: | :---------- |
| launch::async | 0x1(vc) | Asynchronous: The function is called asynchronously by a new thread and synchronizes its return with the point of access to the shared state. |
| launch::deferred | 0x2(vc) | Deferred: The function is called at the point of access to the shared state. |

## Reference and Forwarding
[为什么C++11引入了std::ref？](http://www.cnblogs.com/jiayayao/p/6527713.html)
[C++11尝鲜：右值引用和转发型引用](http://blog.csdn.net/zwvista/article/details/12306283)

