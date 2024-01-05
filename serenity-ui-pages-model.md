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
