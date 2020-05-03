---
layout: post
title:  "Understanding C++ Special Members"
date:   2019-10-14 21:11:04 -0400
categories: c++
---

{% highlight cpp %}
class Base {
public:
  Base()                       { std::puts("default constructor"); }
  Base(Base const&)            { std::puts("copy constructor"); }
  Base(Base&&)                 { std::puts("move constructor"); }
  Base& operator=(Base const&) { std::puts("copy assignment"); }
  Base& operator=(Base&&)      { std::puts("move assignment"); }
  virtual ~Base()              { std::puts("virtual destructor"); }
};

class Derived : public Base {};
{% endhighlight cpp %}

{% highlight cpp %}
void test_special_members() {
  auto dc = Derived{};               // default constructor
  auto cc = dc;                      // copy constructor
  auto mc = std::move(dc);           // move constructor
  auto& ca = cc; ca = dc;            // copy assignment
  auto& ma = mc; ma = std::move(mc); // move assignment
}
{% endhighlight cpp %}

{% highlight cpp %}
int main() {
    test_special_members();
    return 0;
}
{% endhighlight cpp %}

# Class Templates
{% highlight cpp %}
class trivial
{
public:
    normal(); // if it makes sense
    //~normal() = default;
    //normal(const normal&) = default;
    //normal(normal&&) = default;
    //normal& operator=(const normal&) = default;
    //normal& operator=(normal&&) = default;
};
{% endhighlight cpp %}

{% highlight cpp %}
class immoveable
{
public:
    immoveable(); // if it makes sense
    //~immoveable() = default;
    immoveable(const immoveable&) = delete;
    immoveable& operator=(const immoveable&) = delete;
    //immoveable(immoveable&&) = delete;
    //immoveable& operator=(immoveable&&) = delete;
};
{% endhighlight cpp %}

{% highlight cpp %}
class container
{
public:
    container() noexcept;
    ~container() noexcept;
    container(const container& other);
    container(container&& other) noexcept;
    container& operator=(const container& other);
    container& operator=(container&& other) noexcept;
};
{% endhighlight cpp %}

{% highlight cpp %}
class resource_handle
{
public:
    resource_handle() noexcept;
    ~resource_handle() noexcept;
    //resource_handle(const resource_handle&) = delete;
    //resource_handle& operator=(const resource_handle&) = delete;
    resource_handle(resource_handle&& other) noexcept;
    resource_handle& operator=(resource_handle&& other) noexcept;
};
{% endhighlight cpp %}
# Resources
[Howard Hinnant: Move Semantics](https://howardhinnant.github.io/bloomberg_2016.pdf)

[Special Member Functions](https://foonathan.net/2019/02/special-member-functions/)

[Special Member Guidelines](https://foonathan.net/special-member-chart/)

[Abseil TotW #143: C++11 Deleted Functions](https://abseil.io/tips/143)



