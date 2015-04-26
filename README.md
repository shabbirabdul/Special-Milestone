# Special Milestone

Executing an integration build and unit test on the build server is all well and good but before we can 
say a build is complete and ready to go, its a good idea to know if it will work after deployment.

For Special milestone, we are planning to do smoke testing for the deployed application. Some testing is done 
inorder to check 
1. If the application is running 
2. The correct version of code is deployed or not.

Executing a test deployment and then smoke testing will ensure that the archived build is ready to be deployed on
test and production environments.

We used selenium to do smoke testing. Selenium is an open source technology for automating browser based applications. It can also be used to perform load and performance tests.

For this milestone we are using the same project as we used in milestone 3 [https://github.com/spring-projects/greenhouse]

We would like to check if the correct version of software is deployed or not. To do this we are performing a test deploy as shown:
-----image -- use the image we used in presentation

The app subjected to deployment has some changes done on it. The home page of the application was modified. So,the size of the homepage of the new version is different from the previous version. 
-----App before deployment img
-----App after deployment img
 
The jenkins job of the web application is configured in such a way that after successful deployment, It triggers selenium-test job and The selenium test job runs the selenium scripts to test if the correct version of changes are deployed or not.

----- triggering selenium after tesr deployment
 
One away of smoke testing the changes is to check the size of the home page. A selenium script is written to get the size of the page on which changes are made and comparing it with expected size. If the sizes do not match then the test case fails. 

```
		URL local = new URL("http://localhost:9515");
		WebDriver driver = new RemoteWebDriver(local, DesiredCapabilities.chrome());
		// open the browser and go to home page of the application
		driver.get("http://ec2-52-6-55-84.compute-1.amazonaws.com:18080/mywebapp/");
		// wait 5 seconds and close the browser
		Thread.sleep(5000);
		if(driver.getPageSource().length()!=1856)
			fail("Incorrect version of Software deployed");
		driver.quit();

```

Another way to check if the changes made to the web page are deployed is to check if the text we have changed appears on the page. The below code snippet uses Chrome web driver to check if the text "Spring is light weight java application development framework" appears on the page. If the text doesn't appear, the test case fails. 

```
  		URL local = new URL("http://localhost:9515");
		WebDriver driver = new RemoteWebDriver(local, DesiredCapabilities.chrome());
		// open the browser and go to home page of the application
		driver.get("http://ec2-52-6-55-84.compute-1.amazonaws.com:18080/mywebapp/");
		String text = "Spring is light weight java application development framework ";
		if(driver.getPageSource().contains("Spring is light weight java application development   				framework"))
			System.out.println("Correct version of software deployed");
		else
			fail("Incorrect version of Software deployed");
				
```

We have used TestNG plugin to publish selenium results. TestNG is configured as a post-build step in Selenium-Test jenkins job. 

-----Image TestNG 


Selenium results for wrong and correct versions of software deployed.

-----succesful selenium run
-----failure selenium run


If all the selenium test cases are run and the job finishes successfully, we can add a post deploy step to deploy the artifacts in IST or Prod servers.







