# Web Authetication APIs

# Course Website

[FullStack Authentication](https://firtman.github.io/authentication/)

### Slides

[](https://firtman.github.io/authentication/slides.pdf)

# Introduction

## Concepts

95% of this page covers about Authentication and not Authorisation. Authentication has more to do with the verifying identify of a user (or sometimes another system), while Authorisation has to do with permissions that Autheticated user might’ve usually through something called Auth tokens.

The following are come concepts, which we don’t dive into deep but explaining you what they are:

| Credentials | The ones that helps us confirm the identity. For example, a combination of username and password is a good example |
| --- | --- |
| Single Sign On (SSO) | The idea is, we let users login into one web app, let’s say web app A and let them login into web app B automatically. It has more to do authorization — which web apps does the user actually has access to? |
| 2FA ( Two Factor Authentication) | Idea is to have another factor outside of your password, for example, finger print or OTP. |
| MFA (Multi Factor Authentication) | World started with 2FA, but later on many companies added more factors (2 or more) calling it Multi Factor |
| OAuth 2.0  | This also has more to do with Authorization. The idea is about keeping the user logged in for a certain period of time when they are using another server — for example, Facebook Login |
| JWT (JSON Web Token) | The idea is to understand and establish trust regarding the metadata exchanged between two parties |
| OTP (One Time Password) | A common factor these days, triggering OTP to allow users to login |
| Magic Link | It works same as OTP, but instead of user entering numbers, it sends a links (valid for limited time) to use and login without entering any password. |
| Public Key Cryptography | It is a concept of public and private key mathematical pair bound by algorithms. It allows for example, one party to trust the other party if they carry a public key. The trust works because, only private key (not shared with anywone) can crack the partnership and result in trust. |

## Implementing Authetication

There are multiple ways to implementing Authentication in our apps

- ****************Custom Authentication —**************** Usually, we build it ourselves → Username/Password or WebAuthn
- **************************************Identity Providers —************************************** We don’t set up database to to verify, instead we grab it from 3rd party identify providers like Google, Github or others. They give our app the credentials, when user clicks “Login with Google” for example. The restriction, we are expecting user to have a Google Account.
- ******************************************Identity as a service —****************************************** We don’t have to build anything here, everything is taken care by provider.

# Web Authentication Strategies

## HTTP Auth and Pass Strategy

- HTTP Auth is probably used in 90s.
- HTTP passes the passwords over the internet in plain text. A man in the middle can look into them.
- Most of the applications today use Passwordless authentication as follows:

![Untitled](Web%20Authetication%20APIs%202907470dfa9e4002acd55ff5b982569f/Untitled.png)

- User’s biometric acts as a private key while public key is stored in the server or db.
    - Hacker takes public key - he can’t do anything with it.
    - User can’t give private key that easily because it’s not a string to simply by mistake spell it out in human ways.

<aside>
💡 Full password less solution is called Passkeys

The passkey is **not** a label for private key. The passkey is an idea of “removing passwords” for authentication

</aside>

![Untitled](Web%20Authetication%20APIs%202907470dfa9e4002acd55ff5b982569f/Untitled%201.png)

# Classic Login Flow

## Enhancing Login Forms

![Untitled](Web%20Authetication%20APIs%202907470dfa9e4002acd55ff5b982569f/Untitled%202.png)

To give the best user experience to the users as possible, most of the websites don’t follow a simple way. The developers of the websites should have their apps (classic login forms) tell the browser or the password managers by following:

```markdown
// Registration
 autocomplete="new-password"
// Login
 autocomplete="current-password"

Use autocomplete="username", because password managers can use actual email as username, or a different username such as saif_shines. 
```

## Form Accessiblity & UX Practice

```html
<fieldset>
  <label for="register_name">Your Name</label>
  <input
    placeholder="Your Name"
    id="register_name"
    autocomplete="name"
    required
  />
  <label for="register_email">Email</label>
  <input
    placeholder="Your email"
    id="register_email"
    required
    type="email"
    autocomplete="username"
  />
  <label>Your Password</label>
  <input
    type="password"
    id="register_password"
    autocomplete="new-password" 
    required
  />
</fieldset>
```

1. Using `for` is important for the screen readers to understand it’s association with others.
2. The `autocomplete` must be `username` instead of `email` , because it helps password managers to fill in the username correctly, automatically. 
3. Similar for `autocomplete="new-password"` , when registering new users.
4. 

## Credential Management

```jsx
import API from "./API.js";
import Router from "./Router.js";

const Auth = {
  isLoggedIn: false,
  account: null,
  async postLogin(response, user) {
    if (response.ok) {
      this.isLoggedIn = true;
      this.account = user;
      this.updateStatus();
      Router.go("/account");
    } else {
      alert(response.message);
    }
    if (window.PasswordCredential && user.password) {
      const credentials = new PasswordCredential({
        id: user.email,
        password: user.password,
        name: user.name,
      });
      navigator.credentials.store(credentials);
    }
  },
  async register(event) {
    event.preventDefault();
    const user = {
      name: document.getElementById("register_name").value,
      email: document.getElementById("register_email").value,
      password: document.getElementById("register_password").value,
    };
    const response = await API.register(user);
    this.postLogin(response, user);
  },
  async login(event) {
    if (event) event.preventDefault();
    const credentials = {
      email: document.getElementById("login_email").value,
      password: document.getElementById("login_password").value,
    };
    const response = await API.login(credentials);
    this.postLogin(response, {
      ...credentials,
      name: response.name,
    });
  },
  async autoLogin() {
    if (window.PasswordCredential) {
      const credentials = await navigator.credentials.get({ password: true });
      document.getElementById("login_email").value = credentials.id;
      document.getElementById("login_password").value = credentials.password;
      Auth.login();
    }
  },
  logout() {
    this.isLoggedIn = false;
    this.account = null;
    this.updateStatus();
    Router.go("/");
    if (window.PasswordCredential) {
      navigator.credentials.preventSilentAccess();
    }
  },
  updateStatus() {
    if (Auth.isLoggedIn && Auth.account) {
      document
        .querySelectorAll(".logged_out")
        .forEach((e) => (e.style.display = "none"));
      document
        .querySelectorAll(".logged_in")
        .forEach((e) => (e.style.display = "block"));
      document
        .querySelectorAll(".account_name")
        .forEach((e) => (e.innerHTML = Auth.account.name));
      document
        .querySelectorAll(".account_username")
        .forEach((e) => (e.innerHTML = Auth.account.email));
    } else {
      document
        .querySelectorAll(".logged_out")
        .forEach((e) => (e.style.display = "block"));
      document
        .querySelectorAll(".logged_in")
        .forEach((e) => (e.style.display = "none"));
    }
  },
  init: () => {},
};
Auth.updateStatus();
Auth.autoLogin();

export default Auth;

// make it a global object
window.Auth = Auth;
```

The active selection is a JavaScript module called `Auth.js` that is being used in a project that involves JavaScript and `npm`. The module provides functionality for user authentication, including login, registration, and logout.

The `Auth` module contains several properties and methods that are used to manage the user's authentication status and account information. The `isLoggedIn` property is a boolean value that indicates whether the user is currently logged in. The `account` property is an object that contains information about the user's account, such as their name and email address.

The `postLogin()` method is used to handle the response from the server after the user logs in. If the response is successful, the `isLoggedIn` property is set to `true` and the `account` property is set to the user's account information. The method also updates the UI to reflect the user's authentication status and redirects the user to the account page. If the response is not successful, an alert is displayed with an error message.

The `register()` method is used to handle the user's registration form submission. It extracts the user's name, email, and password from the form and sends them to the server using the `API.register()` method. If the registration is successful, the `postLogin()` method is called to log the user in.

The `login()` method is used to handle the user's login form submission. It extracts the user's email and password from the form and sends them to the server using the `API.login()` method. If the login is successful, the `postLogin()` method is called to log the user in.

The `autoLogin()` method is used to automatically log the user in if they have previously saved their credentials using the Credential Management API. It retrieves the saved credentials using the `navigator.credentials.get()` method and calls the `login()` method with the retrieved credentials.

The `logout()` method is used to log the user out. It sets the `isLoggedIn` property to `false`, clears the `account` property, updates the UI to reflect the user's authentication status, and redirects the user to the home page. If the user has saved their credentials using the Credential Management API, the `navigator.credentials.preventSilentAccess()` method is called to prevent silent access to the saved credentials.

Overall, the `Auth` module is a useful tool for managing user authentication in a JavaScript project that uses `npm`. By providing methods for login, registration, and logout, as well as automatic login using the Credential Management API, the module makes it easy for developers to add authentication functionality to their web applications.

# Federated Login

## Sign in with Google

Take this example,

1. I own this website called coffeemasters.com
2. I want users to be able to login into my website. But I don’t want to handle identity verification myself. Instead I want someone else to handle it. So that it makes my life easy as a developer. 
3. So I use Federated Login, where Google or other vendors like google help me in this case. 
4. They verify the user (in what ever means), and tell me **who logged in** so that I can go ahead and continue with my identify based content presentation within my website.

<aside>
💡 Google shares who logged in information, after I share with them a global function that I attach to `window` object during registration.
(Using [Configurator](https://developers.google.com/identity/gsi/web/tools/configurator))

![Untitled](Web%20Authetication%20APIs%202907470dfa9e4002acd55ff5b982569f/Untitled%203.png)

</aside>

[Overview  |  Authentication  |  Google for Developers](https://developers.google.com/identity/gsi/web/guides/overview)

## JWT

I always wondered what is the use of `jwt`.  The internet has always told me it’s to securely transfer the data between two parties. I still wasn’t convinced very well. 

Take the same example above, Google needs to tell my web app, who is logged into my website after it handles identify verification. 

- Well, it can pass JSON object to my website directly through that global function I declare on the `window` object.
- What if some extension or someone else tampers with it? I mean, what if they update the property of `"user":"Saif"` to `"user":"Jones"` ? That’s a risk. A big risk.
- So Google transforms the same information into a special string. It can be which can later be de-transformed into the real data. So even if someone tries to tampers, they can’t tamper to change parts of information, because entire string changes if they try to do that.
- This is what JWT is. It encrypts that information from Google, and can be decrypted by the application and getting use the garuntee of no one in the middle tampered it.

# Web Authentication APIs

![Untitled](Web%20Authetication%20APIs%202907470dfa9e4002acd55ff5b982569f/Untitled%204.png)

![Untitled](Web%20Authetication%20APIs%202907470dfa9e4002acd55ff5b982569f/Untitled%205.png)

![Untitled](Web%20Authetication%20APIs%202907470dfa9e4002acd55ff5b982569f/Untitled%206.png)

Libaries: 

[SimpleWebAuthn](https://simplewebauthn.dev/)