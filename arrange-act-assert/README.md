---
title: Arrange/Act/Assert
category: Idiom
language: en
tag:
 - Testing
---

## Also known as

Given/When/Then

## Intent

Arrange/Act/Assert (AAA) is a pattern for organizing unit tests.
It breaks tests down into three clear and distinct steps:

1. Arrange: Perform the setup and initialization required for the test.
2. Act: Take action(s) required for the test.
3. Assert: Verify the outcome(s) of the test.

排列/行动/断言(AAA)是一种组织单元测试的模式。
它将测试分解为三个清晰而不同的步骤:

1. 安排:执行测试所需的设置和初始化。
2. 行动:采取测试所需的行动。
3. 断言:验证测试的结果。

## Explanation

This pattern has several significant benefits. It creates a clear separation between a test's
setup, operations, and results. This structure makes the code easier to read and understand. If
you place the steps in order and format your code to separate them, you can scan a test and
quickly comprehend what it does.

It also enforces a certain degree of discipline when you write your tests. You have to think
clearly about the three steps your test will perform. It makes tests more natural to write at
the same time since you already have an outline.


这个模式有几个重要的好处。它在测试的设置、操作和结果之间创建了一个清晰的分离。这种结构使代码更容易阅读和理解。如果您将这些步骤按顺序排列，并将代码格式化以将它们分开，那么您就可以扫描一个测试并快速理解它的功能。

它还在编写测试时强制执行一定程度的纪律。您必须清楚地考虑您的测试将执行的三个步骤。因为您已经有了大纲，所以在同一时间编写测试更加自然。

Real world example

> We need to write comprehensive and clear unit test suite for a class.

In plain words

> Arrange/Act/Assert is a testing pattern that organizes tests into three clear steps for easy
> maintenance.

WikiWikiWeb says

> Arrange/Act/Assert is a pattern for arranging and formatting code in UnitTest methods. 

**Programmatic Example**

Let's first introduce our `Cash` class to be unit tested.

```java
public class Cash {

  private int amount;

  Cash(int amount) {
    this.amount = amount;
  }

  void plus(int addend) {
    amount += addend;
  }

  boolean minus(int subtrahend) {
    if (amount >= subtrahend) {
      amount -= subtrahend;
      return true;
    } else {
      return false;
    }
  }

  int count() {
    return amount;
  }
}
```

Then we write our unit tests according to Arrange/Act/Assert pattern. Notice the clearly
separated steps for each unit test.

```java
class CashAAATest {

  @Test
  void testPlus() {
    //Arrange
    var cash = new Cash(3);
    //Act
    cash.plus(4);
    //Assert
    assertEquals(7, cash.count());
  }

  @Test
  void testMinus() {
    //Arrange
    var cash = new Cash(8);
    //Act
    var result = cash.minus(5);
    //Assert
    assertTrue(result);
    assertEquals(3, cash.count());
  }

  @Test
  void testInsufficientMinus() {
    //Arrange
    var cash = new Cash(1);
    //Act
    var result = cash.minus(6);
    //Assert
    assertFalse(result);
    assertEquals(1, cash.count());
  }

  @Test
  void testUpdate() {
    //Arrange
    var cash = new Cash(5);
    //Act
    cash.plus(6);
    var result = cash.minus(3);
    //Assert
    assertTrue(result);
    assertEquals(8, cash.count());
  }
}
```

## Applicability

Use Arrange/Act/Assert pattern when

* You need to structure your unit tests so that they're easier to read, maintain, and enhance. 

## Credits

* [Arrange, Act, Assert: What is AAA Testing?](https://blog.ncrunch.net/post/arrange-act-assert-aaa-testing.aspx)
* [Bill Wake: 3A – Arrange, Act, Assert](https://xp123.com/articles/3a-arrange-act-assert/)
* [Martin Fowler: GivenWhenThen](https://martinfowler.com/bliki/GivenWhenThen.html)
* [xUnit Test Patterns: Refactoring Test Code](https://www.amazon.com/gp/product/0131495054/ref=as_li_qf_asin_il_tl?ie=UTF8&tag=javadesignpat-20&creative=9325&linkCode=as2&creativeASIN=0131495054&linkId=99701e8f4af2f7e8dd50d720c9b63dbf)
* [Unit Testing Principles, Practices, and Patterns](https://www.amazon.com/gp/product/1617296279/ref=as_li_qf_asin_il_tl?ie=UTF8&tag=javadesignpat-20&creative=9325&linkCode=as2&creativeASIN=1617296279&linkId=74c75cf22a63c3e4758ae08aa0a0cc35)
* [Test Driven Development: By Example](https://www.amazon.com/gp/product/0321146530/ref=as_li_qf_asin_il_tl?ie=UTF8&tag=javadesignpat-20&creative=9325&linkCode=as2&creativeASIN=0321146530&linkId=5c63a93d8c1175b84ca5087472ef0e05)
