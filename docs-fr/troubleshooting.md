# Résolution des erreurs

Il est possible que des erreurs surviennent, cela ne vient pas forcément du CMS,
mais voilà les erreurs les plus courantes avec leurs solutions !

## Problèmes courants

### La page d'accueil fonctionne mais les autres pages produisent une erreur 404

La réécriture d'url n'est pas activée, il vous suffit de l'activer (voir question suivante).

### La réécriture d'URL n'est pas activée sous Apache 2
Il faut modifier le fichier `/etc/apache2/sites-available/000-default.conf` et rajouter ces lignes entre les balises `<VirtualHost>`:
```xml
<Directory "/var/www/html">
  AllowOverride All
</Directory>
```

Puis redémarrer Apache2 avec
```
service apache2 restart
```

### Erreur 500 lors de l'inscription

Si le compte est bien créé malgré l'erreur, ce problème peut se produire si
jamais l'envoi des mails n'est pas correctement configuré, pour cela vérifiez
la configuration de l'envoi des mails sur le panel admin de votre site.

### Le fichier n'a pas été téléchargé lors de l'upload d'une image

Ce problème survient lorsque vous uploadez une image dont le poids dépasse le
maximum autorisé par PHP (par défaut 2mo).

Vous pouvez modifier la taille maximum autorisée lors de l'upload dans la configuration
de PHP (dans le `php.ini`) en modifiant les valeurs suivantes:
```
upload_max_filesize = 10M
post_max_size = 10M
```
