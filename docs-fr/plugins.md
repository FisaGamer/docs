# Plugins

## Introduction

Un plugin vous permet d'ajouter de nouvelles fonctionnalités à votre site, de
nombreux plugins sont disponibles sur notre [Market](https://azuriom.com/market)
mais vous pouvez en créer un si vous n'en trouvez pas un qui correspond à vos
besoins.

## Création d'un plugin

Avant de créer un plugin, il est recommandé de lire la
[documentation de Laravel](https://laravel.com/docs/).

### Structuration d'un plugin

```
plugins/  <-- Dossier contenant les différents plugins installés
|  example/  <-- Slug de votre plugin (nom de votre pluin en minuscule)
|  |  plugin.json  <-- Le fichier principal de votre thème contenant les différentes informations
|  |  assets/  <-- Le dossier contenant les assets de votre plugin (css, js, images, svg, etc)
|  |  database/
|  |  | migrations/ <-- Le dossier contenant les migrations de votre plugin
|  |  resources/
|  |  |  lang/  <-- Le dossier contenant les traductions de votre plugin
|  |  |  views/  <-- Le dossier contenant les vues de votre plugin
|  |  routes/ <-- Le dossier contenant les différents routes de votre plugin
|  |  src/ <-- Le dossier contenant les sources de votre plugin
```

### Le fichier plugin.json

Le fichier `plugin.json` est indispensable pour charger un plugin, et
contient les différentes informations d'un plugin:
```json
{
    "name": "Exemple",
    "version": "1.0.0",
    "description": "Un super thème",
    "url" : "https://azuriom.com",
    "authors": [
        "Azuriom"
    ],
    "providers": [
        "\\Azuriom\\Plugin\\Exemple\\Providers\\ExempleServiceProvider",
        "\\Azuriom\\Plugin\\Exemple\\Providers\\RouteServiceProvider"
    ]
}
```

> {info} Pour créer un plugin vous pouvez utiliser la commande suivante qui va
générer automatiquement le dossier du plugin ainsi que de nombreux fichier par
défaut:
```
php artisan plugin:create <nom du plugin>
```

### Routes

Les routes permettent d'asocier une URL à une action en particulier.

Elles sont enregistrées dans le dossier `routes` à la racine du plugin.

Pour plus d'informations sur le fonctionnement des routes vous pouvez lire la
[documentation de Laravel](https://laravel.com/docs/6.x/routing).

```php
Route::get('/support', 'SupportController@index')->name('index');
```

> {warn} Veuillez faire attention à ne pas utiliser de routes avec des closures,
car celles ci ne sont pas compatibles avec certaines optimisations du CMS.

### Vues

Les vues sont la partie visible d'un plugin, ce sont les fichiers content l'HTML
du plugin pour afficher une page.

Azuriom utilisant [Laravel](https://laravel.com/), les vues peuvent être faites en utilisant le moteur
de template Blade. Si vous ne maitrisez pas Blade il est très vivement recommandé
de lire [sa documentation](https://laravel.com/docs/6.x/blade), d'autant plus que celle-ci est assez courte.

> {warn} Il est très vivement recommandé de ne PAS utiliser la syntaxe PHP
traditionnelle lorsque vous travaillez avec Blade, en effet celle-ci n'apporte
aucun avantage et seulement des inconvénients.

Pour afficher une vue vous pouvez utiliser `view('<slug du plugin>::<nom de votre vue>')`,
par exemple `view('support::tickets.index')` pour afficher la vue `tickets.index`
du plugin support.

### Contrôleurs

Les contrôleurs sont une partie centrale d'un plugin, ils se trouvent dans le dossier
`src/Controllers` à la racine du plugin et c'est eux qui s'occuppent
de transformer une reqûete en la réponse qui sera renvoyée à l'utilisateur.

Pour plus d'informations sur le fonctionnement des contrôleurs vous pouvez lire la
[documentation de Laravel](https://laravel.com/docs/6.x/controllers).

```php
<?php

namespace Azuriom\Plugin\Support\Controllers;

use Azuriom\Http\Controllers\Controller;
use Azuriom\Plugin\Support\Models\Ticket;

class TicketController extends Controller
{
    /**
     * Display a listing of the resource.
     *
     * @return \Illuminate\Http\Response
     */
    public function index()
    {
        // On récupère l'ensemble des tickets
        $tickets = Ticket::all();

        // On retourne une vue, en lui passant les tickets
        return view('support::tickets.index', [
            'tickets' => $tickets,
        ]);
    }
}
```

### Modèles

Les modèles représentent une entrée dans une table de la base de données, et permettent
d'intéragir avec la base de données.

Vous pouvez également définir dans un modèle les différentes relations de celui-ci,
par exemple un `Ticket` peut avoir un `User` et une `Category`, et avoir des `Comment`s.

Vous pouvez trouver plus d'informations sur les modèles (aussi appelés Eloquent sur Laravel) dans la
[documentation de Laravel](https://laravel.com/docs/6.x/eloquent).

```php
<?php

namespace Azuriom\Plugin\Support\Models;

use Azuriom\Models\Traits\HasTablePrefix;
use Azuriom\Models\Traits\HasUser;
use Azuriom\Models\User;
use Illuminate\Database\Eloquent\Model;

class Ticket extends Model
{
    use HasTablePrefix;
    use HasUser;

    /**
     * The table prefix associated with the model.
     *
     * @var string
     */
    protected $prefix = 'support_';

    /**
     * The attributes that are mass assignable.
     *
     * @var array
     */
    protected $fillable = [
        'subject', 'category_id',
    ];

    /**
     * The user key associated with this model.
     *
     * @var string
     */
    protected $userKey = 'author_id';

    /**
     * Get the user who created this ticket.
     */
    public function author()
    {
        return $this->belongsTo(User::class, 'author_id');
    }

    /**
     * Get the category of this ticket.
     */
    public function category()
    {
        return $this->belongsTo(Category::class);
    }

    /**
     * Get the comments of this ticket.
     */
    public function comments()
    {
        return $this->hasMany(Comment::class);
    }
}
```

### Service Provider

Les services providers sont le coeur d'un plugin, ils sont appelés à l'initialisation
de Laravel, et permettent d'enregistrer les différentes parties d'un plugin (vues, traductions, middlewares, etc).

Les services providers doivent être ajoutés dans la partie `providers` du `plugins.json`:
```json
{
    "providers": [
        "\\Azuriom\\Plugin\\Support\\Providers\\SupportServiceProvider"
    ]
}
```

Vous pouvez trouver plus d'informations sur les services providers dans la
[documentation de Laravel](https://laravel.com/docs/6.x/providers).

```php
<?php

namespace Azuriom\Plugin\Support\Providers;

use Azuriom\Extensions\Plugin\BasePluginServiceProvider;

class SupportServiceProvider extends BasePluginServiceProvider
{
    /**
     * Register any plugin services.
     *
     * @return void
     */
    public function register()
    {
        $this->registerMiddlewares();

        //
    }

    /**
     * Bootstrap any plugin services.
     *
     * @return void
     */
    public function boot()
    {
        // $this->registerPolicies();

        $this->loadViews();

        $this->loadTranslations();

        $this->loadMigrations();

        $this->registerRouteDescriptions();

        $this->registerAdminNavigation();

        //
    }
}
```


### Migrations

Les migrations permettent de créer, modifier ou supprimer des tables dans la base
de données, elles se trouvent dans le dossier `database/migrations`.

Vous pouvez trouver plus d'informations sur les migrations dans la
[documentation de Laravel](https://laravel.com/docs/6.x/migrations).

```php
<?php

use Illuminate\Database\Migrations\Migration;
use Illuminate\Database\Schema\Blueprint;
use Illuminate\Support\Facades\Schema;

class CreateSupportTicketsTable extends Migration
{
    /**
     * Run the migrations.
     *
     * @return void
     */
    public function up()
    {
        Schema::create('support_tickets', function (Blueprint $table) {
            $table->increments('id');
            $table->string('subject');
            $table->unsignedInteger('author_id');
            $table->unsignedInteger('category_id');
            $table->timestamp('closed_at')->nullable();
            $table->timestamps();

            $table->foreign('author_id')->references('id')->on('users');
            $table->foreign('category_id')->references('id')->on('support_categories');
        });
    }

    /**
     * Reverse the migrations.
     *
     * @return void
     */
    public function down()
    {
        Schema::dropIfExists('support_tickets');
    }
}
```

### Traductions

Les traductions permettent de traduire un plugin (incroyable), elles se trouvent
dans le dossier `resources/lang` à la racine d'un plugin, dans le dossier de la 
langue (`en`, `fr`, etc)

Vous pouvez trouver plus d'informations sur les traductions dans la
[documentation de Laravel](https://laravel.com/docs/6.x/localization).

Pour récupérer une traduction vous pouvez utiliser la fonction
`trans('<nom de votre plugin>::<nom du fichier>.<message>`), par exemple
`trans('support::messages.tickets.home')` pour afficher le message `tickets.home`,
dans le fichier `messages.php` du plugin support:
```php
<?php

return [
  'tickets' => [
    'home' => 'Your tickets',
  ],
];
```