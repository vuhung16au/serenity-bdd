# Overview

How to use UI Pages Models with Serenity BDD

# UI Layer 

LoginUI.java

```java
package com.vuhung.seleniium.ui;

import org.openqa.selenium.support.FindBy;

import net.serenitybdd.annotations.DefaultUrl;

@DefaultUrl("http://localhost:8000/wp-login.php")
public class LoginUI {

	public static final String INPUT_ID_USER_LOGIN = "//input[@id='user_login']";
	public static final String INPUT_ID_USER_PASSWORD = "//input[@id='user_pass']"; 
	
}

```

# Page Layer
LoginPage.java

```java
package com.vuhung.seleniium.pages;

import org.openqa.selenium.By;
import org.openqa.selenium.support.FindBy;

import com.vuhung.seleniium.model.WordpressUser;
import com.vuhung.seleniium.ui.LoginUI;

import net.serenitybdd.annotations.DefaultUrl;
import net.serenitybdd.core.pages.PageObject;
import net.serenitybdd.core.pages.WebElementFacade;

@DefaultUrl("http://localhost:8000/wp-login.php")
public class LoginPage extends PageObject {
	
	public void EnterUsername(String username) { 
		$(LoginUI.INPUT_ID_USER_LOGIN).sendKeys(username);
	}

	public void EnterPassword(String password) { 
		// LoginUI.userElement.sendKeys(password);
		$(LoginUI.INPUT_ID_USER_PASSWORD).sendKeys(password);
		

	}

	public void ClickLoginButton() { 
		$(By.id("wp-submit")).click();
	}

	public void asNormalUser(WordpressUser user) {
		
		open();
		
		EnterUsername(user.getUserName());
		EnterPassword(user.getPassword());
		ClickLoginButton();
	}

}

```

# Model Layer
Wordpress.java

```java
package com.vuhung.seleniium.model;

public class WordpressUser {
	private String userName;
	private String password; 

	/**
	 * @return the userName
	 */
	public String getUserName() {
		return userName;
	}
	/**
	 * @param userName the userName to set
	 */
	public void setUserName(String userName) {
		this.userName = userName;
	}
	/**
	 * @return the password
	 */
	public String getPassword() {
		return password;
	}
	/**
	 * @param password the password to set
	 */
	public void setPassword(String password) {
		this.password = password;
	}
	/**
	 * @param userName
	 * @param password
	 */
	public WordpressUser(String userName, String password) {
		this.userName = userName;
		this.password = password;
	}
	
}

``` 

# The test code 

```java

	@Test 
	public void loginValidUsernamePassword() {
				
		String userName = "vuhung";
		String userPassword = "vuhung";
		WordpressUser user = new WordpressUser(userName, userPassword);

		loginPage.asNormalUser(user);
		
		String expectedAfterLoginPageTitle = "Dashboard ‹ oz wordpress — WordPress";
		String actualAfterLoginPageTitle = loginPage.getTitle()
    assertThat(expectedAfterLoginPageTitle, equalTo(actualAfterLoginPageTitle));

	}

```

# Test Run and the Report 

```
vhmac:selenium-itnews vuhung$ mvn verify
[INFO] Scanning for projects...
[INFO]
[INFO] ----------------< com.vuhung.seleniium:selenium-itnews >----------------
[INFO] Building Serenity project with JUnit and WebDriver 1.1
[INFO]   from pom.xml
[INFO] --------------------------------[ jar ]---------------------------------
[INFO]
[INFO] --- resources:3.3.1:resources (default-resources) @ selenium-itnews ---
[INFO] skip non existing resourceDirectory /Users/vuhung/eclipse-workspace/selenium-itnews/src/main/resources
[INFO]
[INFO] --- compiler:3.2:compile (default-compile) @ selenium-itnews ---
[INFO] No sources to compile
[INFO]
[INFO] --- resources:3.3.1:testResources (default-testResources) @ selenium-itnews ---
[INFO] Copying 1 resource from src/test/resources to target/test-classes
[INFO]
[INFO] --- compiler:3.2:testCompile (default-testCompile) @ selenium-itnews ---
[INFO] Changes detected - recompiling the module!
[INFO] Compiling 17 source files to /Users/vuhung/eclipse-workspace/selenium-itnews/target/test-classes
[INFO] /Users/vuhung/eclipse-workspace/selenium-itnews/src/test/java/com/vuhung/assertj/AssertJCoreUnitTest.java: /Users/vuhung/eclipse-workspace/selenium-itnews/src/test/java/com/vuhung/assertj/AssertJCoreUnitTest.java uses or overrides a deprecated API.
[INFO] /Users/vuhung/eclipse-workspace/selenium-itnews/src/test/java/com/vuhung/assertj/AssertJCoreUnitTest.java: Recompile with -Xlint:deprecation for details.
[INFO]
[INFO] --- surefire:3.0.0-M5:test (default-test) @ selenium-itnews ---
[INFO] Tests are skipped.
[INFO]
[INFO] --- jar:3.3.0:jar (default-jar) @ selenium-itnews ---
[INFO]
[INFO] --- failsafe:3.0.0-M5:integration-test (default) @ selenium-itnews ---
[INFO]
[INFO] -------------------------------------------------------
[INFO]  T E S T S
[INFO] -------------------------------------------------------
[INFO] Running com.vuhung.assertj.AssertJCoreUnitTest
[INFO] Tests run: 7, Failures: 0, Errors: 0, Skipped: 0, Time elapsed: 0.186 s - in com.vuhung.assertj.AssertJCoreUnitTest
[INFO] Running com.vuhung.seleniium.features.login.AuthorizationStory
[main] INFO  -

-------------------------------------------------------------------------------------
     _______. _______ .______       _______ .__   __.  __  .___________.____    ____
    /       ||   ____||   _  \     |   ____||  \ |  | |  | |           |\   \  /   /
   |   (----`|  |__   |  |_)  |    |  |__   |   \|  | |  | `---|  |----` \   \/   /
    \   \    |   __|  |      /     |   __|  |  . `  | |  |     |  |       \_    _/
.----)   |   |  |____ |  |\  \----.|  |____ |  |\   | |  |     |  |         |  |
|_______/    |_______|| _| `._____||_______||__| \__| |__|     |__|         |__|

 News and tutorials at https://serenity-bdd.info
 Documentation at https://serenity-bdd.github.io
 Test Automation Training and Coaching: https://www.serenity-dojo.com
 Commercial Support: https://www.serenity-dojo.com/serenity-bdd-enterprise-support
 Join the Serenity Community on Gitter: https://gitter.im/serenity-bdd/serenity-core
-------------------------------------------------------------------------------------

[main] INFO  - Test Suite Started: Authorization story
[main] INFO  -
  _____   ___   ___   _____     ___   _____     _     ___   _____   ___   ___
 |_   _| | __| / __| |_   _|   / __| |_   _|   /_\   | _ \ |_   _| | __| |   \
   | |   | _|  \__ \   | |     \__ \   | |    / _ \  |   /   | |   | _|  | |) |
   |_|   |___| |___/   |_|     |___/   |_|   /_/ \_\ |_|_\   |_|   |___| |___/

loginValidUsernamePassword
--------------------------------------------------------------------------------
[main] INFO  -
  _____   ___   ___   _____     ___     _     ___   ___   ___   ___
 |_   _| | __| / __| |_   _|   | _ \   /_\   / __| / __| | __| |   \
   | |   | _|  \__ \   | |     |  _/  / _ \  \__ \ \__ \ | _|  | |) |
   |_|   |___| |___/   |_|     |_|   /_/ \_\ |___/ |___/ |___| |___/

Login valid username password
----------------------------------------------------------------------
[INFO] Tests run: 1, Failures: 0, Errors: 0, Skipped: 0, Time elapsed: 12.842 s - in com.vuhung.seleniium.features.login.AuthorizationStory
[INFO]
[INFO] Results:
[INFO]
[INFO] Tests run: 8, Failures: 0, Errors: 0, Skipped: 0
[INFO]
[INFO]
[INFO] --- serenity:4.0.29:aggregate (serenity-reports) @ selenium-itnews ---
[INFO] GENERATING REPORTS FOR: /Users/vuhung/eclipse-workspace/selenium-itnews
[INFO] GENERATING REPORTS USING 32 THREADS
[INFO] GENERATING SUMMARY REPORTS...
[INFO] GENERATING REQUIREMENTS REPORTS...
[INFO] GENERATING RESULT REPORTS...
[INFO] GENERATING ERROR REPORTS...
[INFO] Test results for 1 tests generated in 1.2 secs in directory: file:/Users/vuhung/eclipse-workspace/selenium-itnews/target/site/serenity/
[INFO] ------------------------------------------------
[INFO] | SERENITY TESTS:               | SUCCESS
[INFO] ------------------------------------------------
[INFO] | Test scenarios executed       | 1
[INFO] | Total Test cases executed     | 1
[INFO] | Tests passed                  | 1
[INFO] | Tests failed                  | 0
[INFO] | Tests with errors             | 0
[INFO] | Tests compromised             | 0
[INFO] | Tests aborted                 | 0
[INFO] | Tests pending                 | 0
[INFO] | Tests ignored/skipped         | 0
[INFO] ------------------------------- | --------------
[INFO] | Total Duration| 10s 536ms
[INFO] | Fastest test took| 10s 536ms
[INFO] | Slowest test took| 10s 536ms
[INFO] ------------------------------------------------
[INFO]
[INFO] SERENITY REPORTS
[INFO]   - Full Report: file:///Users/vuhung/eclipse-workspace/selenium-itnews/target/site/serenity/index.html
[INFO]
[INFO] --- failsafe:3.0.0-M5:verify (default) @ selenium-itnews ---
[INFO] ------------------------------------------------------------------------
[INFO] BUILD SUCCESS
[INFO] ------------------------------------------------------------------------
[INFO] Total time:  22.419 s
[INFO] Finished at: 2024-01-05T12:07:52+11:00
[INFO] ------------------------------------------------------------------------
``` 
