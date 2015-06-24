# Mercado Pago SDK for Laravel

* [Instalar](#install)
* [Configurando](#config)
* [Como usar](#how-to)
* [Mais informações](#info)

<a name="install"></a>
### Instalar

`composer require livepixel/mercado-pago`

No seu arquivo `config/app.php` adicione o seguinte trecho de código:

```php
'providers' => [

    /*
     * Laravel Framework Service Providers...
     */

    'LivePixel\MercadoPago\Providers\MercadoPagoServiceProvider',
],
``` 

Você também pode criar um `alias` com o trecho de código:

```php
'aliases' => [
	// Outros alias 

    'MP' => 'LivePixel\MercadoPago\Facades\MP',
]
```

<a name="config"></a>
### Configurando

Antes de começar a usar vamos publicar o arquivo de configuração. Na pasta do seu projeto Laravel, execute o seguinte comando artisan:

`php artisan vendor:publish`

O comando acima irá gerar um arquivo `config/mercadopago.php`. Neste arquivo você deve adicionar seu App Id e App Secret. Para saber qual é o seu acesse o [site do Mercado Pago](https://www.mercadopago.com/mlb/ferramentas/aplicacoes)

```php
return [
	'app_id'     => env('MP_APP_ID', 'SEU CLIENT ID'),
	'app_secret' => env('MP_APP_SECRET', 'SEU CLIENT SECRET')
];
```

Você também pode configurar adicionando as chaves `MP_APP_ID` e `MP_APP_SECRET` em seu arquivo `.env` (recomendado).

<a name="how-to"></a>
### Como usar

Neste exemplo, vamos criar uma preferência de pagamento e depois redirecionar o usuário para realizar o pagamento no Mercado Pago.

```php
<?php

namespace App\Http\Controllers;

use Illuminate\Http\Request;

use App\Http\Requests;
use App\Http\Controllers\Controller;
use MP;

class HomeController extends Controller
{
    /**
     * Display a listing of the resource.
     *
     * @return Response
     */
    public function index()
    {
        $preference_data = array (
            "items" => array (
                array (
                    "title" => "Test2",
                    "quantity" => 1,
                    "currency_id" => "BRL",
                    "unit_price" => 10.41
                )
            )
        );

        try {
            $preference = MP::create_preference($preference_data);
            return redirect()->to($preference['response']['init_point']);
        } catch (Exception $e){
            dd($e->getMessage());
        }
    }
}
```

<a name="info"></a>
### Mais informações

Para mais informações acesse o site do [Mercado Pago para desenvolvedores](https://developers.mercadopago.com/) e também o [repositório do SDK oficial](https://github.com/mercadopago/sdk-php)
