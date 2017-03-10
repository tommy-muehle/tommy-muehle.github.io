---
layout: post
title:  "Never implement tests in abstract test cases"
introduction: "My opinion about forcing tests over multiple test-suites or cases."
---

Yesterday I saw an abstract test case with concrete tests for all child test classes.
Something like this:

```
abstract class MyTestCase extends \PHPUnit_Framework_TestCase
{
    private $object;

    /**
     * Awesome test, must work in every child test class
     */
    public function testCanCheckMethod()
    {
        ...
        $this->assertEquals($this->object...);
    }
}

class RealTest extends MyTestCase
{
    // further specific tests with $this->object
    ...
}

```

## A better way

But if you want that all child test classes execute one or more specific test(s),
implement abstract tests like this:

```
abstract class MyTestCase extends \PHPUnit_Framework_TestCase
{
    private $object;

    /**
     * Awesome test, must work in every child test class
     */
    abstract public function testCanCheckMethod();
}

class RealTest extends MyTestCase
{
    public function testCanCheckMethod()
    {
        ...
        $this->assertEquals($this->object...);
    }
}

```

## Why

You definitely come to the point where the tests from the parent test case won't work for you anymore
and the only thing you can do is to override. And that is an unnecessary complexity in your testsuite
and makes no sense.

Making the needed general test abstract gives you the option to implement it concrete for the child test class
or skip it with a meaningful message if the test is not useful in this child test class.

Another point is that concrete tests for one class over more than one file are bad for readability and
to fully understand how it works can take more time.
