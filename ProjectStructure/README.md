#Creditmantri Test automation frame work 

##features
*Language:java
*Type of framework: Data driven framework by using page object model design pattren with page factory
*Database: heidiSQL
*Build management tool: maven
*Web services :httpClient
*TestNG tool
*Test/Defect management tool: Mingle
*CI tool: jenkins
*Version control: gitlab
*Reports: Extent reports
*Cloud services: Browser stack
*Can automate desktop web on different platforms on different browsers
*Can automate mobile web for responsive testing on different devices on different browsers
*Can automate android and ios app on different devices with different versions
*Can automate rest web services by using HTTP client
*Reading web elements from JSON file or java class
*Reading test data from JSON file or excel files
*Integrated this frame work with heidiSQL Database 
*Can execute required test cases from control sheet
*Can execute required test cases on different environments and passing this environment file from the TestNG_XML_FromControlSheet class
*Integrated this frame work with Docker and selenium grid for parallel testing

##4 major challenge class in our project 

**with out tester help any person can execute any lender product on specific environment(uat, production, pre production)
public class AnyProductRunner {

	public static void main(String[] args) {
	
	In this method we are executing test.xml for specific class, for any grid like SBI-cc, HDFC-pl, RBL-gl just we update data in the specific grid excel sheet and call the respective grid test case and add to testng.xml file developer or BA or product engineer any one can easily execute specific product with out tester help with specific environment like qa, ps, production
		
	}

}

====================================================================================

** Execute required test cases from control sheet
public class RunTestNG_XML_FromControlSheet {

	This method we are calling in pom.xml file and we execute pom.xml file through .bat file
	In this main method we are passing .properties file dynamically for different environments 
	like qa,ps,production and execution Reports will generate respective environment
	 We are creating this class as a jar file
	
	public static void main(String[] args) {
		
		ControlSheetReader suite = new ControlSheetReader("./AutomationControlSheet.xlsx");
		suite.getTests("select * from TestCase where Active ='y' and module='cc'");
		// Create object of TestNG Class
		TestNG runner=new TestNG();
		//create Listener objects
		TestListner testListner= new TestListner();
		RetryListener retryListener= new RetryListener();
		// Create a list of String 
		List<String> suitefiles=new ArrayList<String>();
		// Add xml file which you have to execute
		suitefiles.add("./testng.xml");
		//Add listener classes to xml file
         runner.addListener(testListner);
         runner.addListener(retryListener);
		// now set xml file for execution
		runner.setTestSuites(suitefiles);
		// finally execute the runner using run method
		runner.run();
	

}
}

===========================================================================================

**Test data grid maintenance is very big challenge in every project because we have so many lenders like sbi, hdfc, rbl, yes bank, sriram, tata
public class DataProviderClass  {


	here we are maintain all sheets data, no need to write @DataProvider in every test cases
	
	@DataProvider(name="login")
	public static Object[][] loginData() throws Exception {
		Object data[][] = ReadExcelData.excelReader(Repository.getProperty("login_excelSheetPath"), "LoginMobileNum");
		return data;
	}
	
	@DataProvider(name="registration")
	public static Object[][] registrationData() throws Exception {
		Object data[][]=ReadExcelData.excelReader(Repository.getProperty("registration_excelSheetPath"),
		 "registration");
		return data;
	}
	
	@DataProvider(name="SBI_CC")
	public static Object[][] sbiCCData() throws Exception {
		Object data[][] = ReadExcelData.excelReader(Repository.getProperty("cc_excelSheetPath"), "sbi");
		return data;
	}
	@DataProvider(name="HDFC_CC")
	public static Object[][] hdfcCCData() throws Exception {
		Object data[][] = ReadExcelData.excelReader(Repository.getProperty("cc_excelSheetPath"),"hdfc");
		return data;
	}


}

====================================================================


***Execute tests in all environments(uat, preproduction, production) also one big challenge

@Sources({
    "classpath:${env}.properties"
})
public interface ReadSpecificConfigFileData  {

    String url();

    String username();

    String password();  
    
   
 }

