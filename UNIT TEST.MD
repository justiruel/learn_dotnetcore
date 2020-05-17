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
using Newtonsoft.Json;
using Xunit;
/*
 https://docs.microsoft.com/en-us/aspnet/web-api/overview/testing-and-debugging/unit-testing-with-aspnet-web-api
 https://www.youtube.com/watch?v=ub3P8c87cwk&t=796s
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
        [Trait("Category", "Unit1")]
        [InlineData(0, "Hello A")]
        [InlineData(1, "Hello B")]
        public void SayHiTest(int input1, string expected)
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

        [Fact]
        [Trait("Category", "Unit2")]
        public void ReturnObjectTest()
        {
            // Arrange
            ClassTest account = new ClassTest();
            string expected;
            string actual1;
            bool actual2;


            // Act
            var jsonResult = account.ReturnObject(1);
            var dynData = JsonConvert.DeserializeObject<dynamic>(jsonResult);
            expected = "coba 1";
            actual1 = (string)dynData.FullName;
            actual2 = (bool)dynData.TimerStatus;

            // Assert
            Assert.Equal(expected, actual1);
            Assert.True(actual2);

            //Assert.Equal(expected, actual);
            //Assert.IsTrue((bool)dynData.ok);

        }
    }
}
```
