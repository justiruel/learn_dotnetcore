# UNIT TEST
- install nugget
```
xunit
xunit.runner.visualstudio
xunit.runner.console
```
- klik kanan pada method -> create unit test
- copy this to method test
```
//using Microsoft.VisualStudio.TestTools.UnitTesting;
using Xunit;
/*
 * https://www.youtube.com/watch?v=ub3P8c87cwk&t=796s
https://www.c-sharpcorner.com/article/a-basic-introduction-of-unit-test-for-beginners/
The motto of Test-Driven Development is Red, Green, Refactor.
Red: Create a test and make it fail.
Green: Make the test pass by any means necessary.
Refactor: Change the code to remove duplication in your project and to improve the design while ensuring that all tests still pass.*/

namespace ConsoleApp2.Tests
{
    public class ClassTestTests
    {
        [Theory]
        [Trait("Category", "Unit")]
        [InlineData(0, "Hello A")]
        [InlineData(1, "Hello B")]
        public void SayHiTest(int input1,string expected)
        {
            // Arrange
            ClassTest account = new ClassTest();
            //string expected = "Hello B";
            string actual;

            // Act
            actual = account.SayHi(input1);

            // Assert
            Assert.Equal(expected, actual);

            //cara lain
            //Assert.Contains("hello".ToLower(), actual.ToLower());
        }
    }
}
```
