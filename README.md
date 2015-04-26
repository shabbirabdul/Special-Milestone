# Special Milestone

Executing an integration build and unit test on the build server is all well and good but before we can 
say a build is complete and ready to go, its a good idea to know if it will work after deployment.

For Special milestone, we are planning to do smoke testing for the deployed application. Some testing is done 
inorder to check 
1. If the application is running 
2. The correct version of code is deployed or not.

Executing a test deployment and then smoke testing will ensure that the archived build is ready to be deployed on 
test and production environments.

We used selenium to do smoke testing. Selenium is an open source technology for automating browser based applications.
It can also be used to perform load and performance tests.

For this milestone we are taking the same project as we used in milestone 3.
https://github.com/spring-projects/greenhouse

We would like to check if the correct version of software is deployed or not. To do this we are performing a test deploy as shown:
##image

The app subjected to deployment has some changes done on it. Like, the home page was modified. So,the size of the homepage of the new version is different from the previous version. 
one away of smoke testing is to check the size of the home page. A selenium script is written to do the same. 
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

```

  URL local = new URL("http://localhost:9515");
			WebDriver driver = new RemoteWebDriver(local, DesiredCapabilities.chrome());
			// open the browser and go to home page of the application
			driver.get("http://ec2-52-6-55-84.compute-1.amazonaws.com:18080/mywebapp/");
			String text = "Spring is light weight java application development framework ";
			if(driver.getPageSource().contains("Spring is light weight java application development framework"))
				System.out.println("Correct version of software deployed");
			else
				fail("Incorrect version of Software deployed");
				
```

We have used TestNG plugin to publish results.














