src 
	- main
	- test

public class CalculatorTest {
	@Test
	void twoPlusTwoShouldEqualFour(){
			ClassToBeTested calculator = new ClassToBeTested();
			
			assertEquals(4, calculator.methodToBeTestedLikeAddition(2, 2));
			// 4 is expected value
	
			// assertNotEquals, assertTrue, assertFalse, assertNull, assertNotNull	
	}
}

If method to be tested will throw exception, you can just say:
	calculator.methodToBeTested("testArgument");
Or you can use (to test if method throws IllegalArgumentException):
	assertThrows(IllegalArgumentException.class, 
		() -> {
			calculator.methodToBeTested("testArgument");
		});
