# AzAuth

AzAuth is an api that allows to authenticate users of a site under Azuriom on any platform.

## Use of AzAuth (Java)

### Using with [OpenLauncherLib](https://github.com/Litarvan/OpenLauncherLib/) _(for minecraft launcher)_

To begin, add AzAuth as a dependency to your project.
Also if you are using [OpenAuth](https://github.com/Litarvan/OpenAuth/), it is recommended that you remove it,
although it does not cause any real problems, it is no longer used if you use AzAuth.

You should have in the code of your launcher an `aut` method similar to the code below:
```java
	public void auth(String username, String password) throws AuthenticationException {
		Authenticator authenticator = new Authenticator(Authenticator.MOJANG_AUTH_URL, AuthPoints.NORMAL_AUTH_POINTS);
		AuthResponse response = authenticator.authenticate(AuthAgent.MINECRAFT, username, password, "");
		authInfos = new AuthInfos(response.getSelectedProfile().getName(), response.getAccessToken(), response.getSelectedProfile().getId());
	}
```
You just have to replace it by the code below, to modify `<url>` by the url at the root of your site under azuriom.
```java
	public void auth(String username, String password) throws AuthException, IOException {
		AzAuthenticator authenticator = new AzAuthenticator("<url>");
		authInfos = authenticator.authenticate(username, password, AuthInfos.class);
	}
```
Once this is done, AzAuth is integrated into your launcher.

### Using without OpenLauncherLib _(normal usage)_

AzAuth has been designed with [Gson](https://github.com/google/gson) as its only dependency, so you can use it perfectly well if you don't use
OpenLauncherLib, you can simply use `AzAuthenticator#authenticate(String username, String password)` and that
give you directly an `AuthResult` containing the nickname, the uuid and the access token.