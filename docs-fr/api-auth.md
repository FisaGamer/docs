# AzAuth

AzAuth est une api qui permet d'authentifier les utilisateurs d'un site sous Azuriom sur n'importe quelle plateforme
(comme par exemple un launcher Minecraft personnalisé).

## Téléchargement

Vous pouvez télécharger les sources et les releases d'AzAuth sur
[GitHub](https://github.com/Azuriom/AzAuth/releases).

Si vous utilisez un gestionnaire de dépendances, vous pouvez ajouter AzAuth comme
dépendance de la facon suivante:

> {warn} Le repo Maven n'est pas encore disponible mais il le sera très prochainement !

### Gradle

Dans le `build.gradle`:
```groovy
repositories {
    mavenCentral()
}
```
```groovy
dependencies {
    implementation 'com.azuriom:azauth:1.0.0-SNAPSHOT'
}
```

### Maven

Dans le `pom.xml`:
```xml
<dependencies>
    <dependency>
        <groupId>com.azuriom</groupId>
        <artifactId>azauth</artifactId>
        <version>1.0.0-SNAPSHOT</version>
        <scope>compile</scope>
    </dependency>
</dependencies>
```

## Utilisation de AzAuth (Java)

Avant d'utiliser AzAuth, veuillez vérifier que l'api est bien activée en allant
dans les réglages de votre site, sur votre panel admin.

### Utilisation avec [OpenLauncherLib](https://github.com/Litarvan/OpenLauncherLib/) _(pour launcher minecraft)_

Pour commencer, ajoutez AzAuth en dépendance à votre projet.
Également si vous utilisez [OpenAuth](https://github.com/Litarvan/OpenAuth/), il est recommandé de le retirer,
bien que ne causant pas de réels problèmes, il ne sera simplement plus utilisé si vous utilisez AzAuth.

Vous devriez avoir dans le code de votre launcher une méthode `auth` ressemblant au code ci-dessous:
```java
public void auth(String username, String password) throws AuthenticationException {
    Authenticator authenticator = new Authenticator(Authenticator.MOJANG_AUTH_URL, AuthPoints.NORMAL_AUTH_POINTS);
    AuthResponse response = authenticator.authenticate(AuthAgent.MINECRAFT, username, password, "");
    authInfos = new AuthInfos(response.getSelectedProfile().getName(), response.getAccessToken(), response.getSelectedProfile().getId());
}
```
Il vous suffit de la remplacer par le code ci dessous, en remplaçant `<url>` par l'url de la racine de votre site sous Azuriom.
```java
public void auth(String username, String password) throws AuthException, IOException {
    AzAuthenticator authenticator = new AzAuthenticator("<url>");
    authInfos = authenticator.authenticate(username, password, AuthInfos.class);
}
```
Une fois ceci fait, AzAuth est intégré à votre launcher.

### Utilisation sans OpenLauncherLib

AzAuth a été conçu avec comme seule dépendance [Gson](https://github.com/google/gson), vous pouvez donc parfaitement l'utiliser si vous n'utilisez pas
OpenLauncherLib, vous pouvez simplement utiliser `AzAuthenticator#authenticate(String username, String password)` et cela va
vous donner directement un `User` contenant le pseudo, l'uuid, le grade, l'access token et pleins d'autres données utiles.
