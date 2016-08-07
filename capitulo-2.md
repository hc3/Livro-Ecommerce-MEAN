#Capitulo 2

## Construindo uma frente de loja com AngularJS

Vamos usar o AngularJS para construir o front-end, o angularJS é um framework muito conhecido que surgiu em 2009 e ganhou
a graça dos desenvolvedores por ser muito completo e ideal para aplicações SPA ( single page applications ), o angularJS é
um framework MVC ( model view controller ), nesse capitulo vamos ver o seguinte:

<ul>
  <li>Entendendo o AngularJS e a estrutura de pastas do cliente</li>
  <li>Projeto visual do site e-commerce</li>
  <li>Administrando produtos (CRUD)</li>
</ul>

### Entendendo o AngularJS e a estrutua de pastas do cliente.

vamos rodar o grunt com <b>grunt serve</b> ( lembrando que devemos estar com o servidor do mongo online <b>mongod</b> ), vamos
observar a estrutura de pastas criadas pelo Yeoman, temos três páginas principais sendo elas <b>client</b>, <b>server</b>
,<b>e2e</b>. Nesse capitulo vamos focar em client que é onde vão estar os códigos do angularJS e itens como imagens, fontes
entre outros... dentro da pasta cliente vamos ter a pasta <b>app</b> onde vão estar os códigos do angularJS dividos da seguinte
maneira:
root/client/app/cliente
<ul>
  <li>cliente.js - <b>Rotas</b></li>
  <li>cliente.controller.js - <b>Controladores</b></li>
  <li>cliente.controller.spec.js - <b>Testes</b></li>
  <li>cliente.html - <b>Visão</b></li>
  <li>cliente.css - <b>Folha de estilos</b></li>
</ul>

#### Diretivas

As diretivas são extenssões HTML na forma de atributos, tags, classes CSS e até comentários. as diretivas são um grande 
diferencial do angularJS pois com elas conseguimos estender as funcionalidades do html e podemos até mesmo criar nossas
próprias diretivas. A diretiva ng-app é necessária para inciiar o angularJS automaticamente, ela define o modulo raiz.

##### Módulos

Os módulos são o meio preferido para organizar o código em projetos do angularJS. o angularJS fornece uma variável global
chamada angular e ela tem a função module a função module será utilizada para configurar, chamar módulos e administrar nossas
dependências, angular.module pode ser chamado como um getter ou setter:

angular.module('nomeDoModulo') -> getter
angular.module('nomeDoModulo',['dependência']) -> setter

quando o angular se deparar com o nomeDoModulo ele já vai saber que terá que executar um módulo com esse nome e deixar 
suas dependências disponíveis, o getter angular.module tem um grande número de métodos que podem ser encadeados a ele 
como config, factory, run e etc...

##### Roteamento no AngularUI

A funcionalidade de roteamento permite que o usuário tenha uma URL que reflita o estado atual da aplicação. Normalmente
uma aplicação SPA pode ter inúmeras páginas diferentes em um único e imutável URL, para contornar esse problema usaremos
um módulo chamado ui-router, que associará URLs únicos a estados específicos da aplicação, podemos ver um exemplo:

````js
angular.module('meanshopApp')
  .config(function ($stateProvider) {
    $stateProvider
      .state('main', {
        url: '/',
        templateUrl: 'app/main/main.html',
        controller: 'MainCtrl'
      });
  });
````

O roteador do angularJS UI permite definir views e estados. Rotas, controladores e estados são associados e atemplates HTML
por meio de $stateProvider cria o URL raiz ('/') e o associa a um template (main.html) e a um controlador (MainCtrl) um padrão
semelhante será seguido por todos os arquivos de rotas.

##### Controladores e escopos

Como em qualquer framework MVC, um controlador interage com as views e os módulos. os controladores são responsáveis pela
carga dos dados e sua representação nos templates HTML ( ou seja as view ), vamos ver um exemplo de controlador.

````js
angular.module('meanshopApp')
  .controller('MainCtrl', function ($scope, $http, socket) {
    $scope.awesomeThings = [];
    
    $http.get('/api/things')
      .success(function(awesomeThings) {
        $scope.awesomeThings = awesomeThings;
    });
  })

````

o que é $scope? É um objeto que "cola" o controlador ás views, providenciando o two way data binding ( cada vez que uma vairável
é atualizada no $scope a alteração é automaticamente renderizada no HTML.

##### Controladores e escopos

Os templates são arquivos html enriquecidos com aprimoramentos do angularJS, templates são partes do código que vão estar em arquivos
separados para que possamos reaproveitar o código HTML sempre que preciso.

### Criando um o layout do MVP do e-commerce.

vamos agora criar nosso próprio layout.

#### Produtos

Nessa página, todos os produtos da loja serão exibidos. Usaremos o gerador Yeoman para automatizar a crição dos arquivos

<b> yo angular-fullstack:route products </b>

#### Fábricas e serviços

em angularJS serviços são objetos ou funções únicas vinculados a controladores ou outros componentes usando Injeção de dependência
a tarefa do controlador é vincular os dados com a a view dentro de um $scope. Por outro lado os serviços fazem o trabalho complicado
de obter e repassar esses dados. <b> os serviços podem ser chamados de qualquer lugar não apenas dos controladores, mas também de diretivas,
 filtros e onde mais for preciso </b>.

<ul>
  <li><b>Serviços</b>: a palavra-chave this assume a instância da função. Os serviços retornam um construtor de função e, portanto, é necessário
  usar o operador new</li>
  <li><b>Fábricas</b>: a palavra-chave this assume o valor devolvido pela função ao ser chamada. Permite criado <b>closures</b></li>
</ul>

##### Criando a fábrica de produtos.

quando criamos uma factory o this assume o valor devolvido pela função ao ser chamada. Permite criar closures.

vamos usar novamente o gerador Yeoman:

<b> yo angular-fullstack:factory products </b>

vamos usar por enquanto dados em memória vamos acessar products.service.js.

/* client/app/products/products.service.js */

````js
angular.module('meanshopApp')
  .factory('Product', function () {

    return [
      {_id: 1, title:'Product 1', price:123.45, quantity: 10, description:'Lorem ipsum dolor'},
      {_id: 2, title:'Product 2', price:123.45, quantity: 10, description:'Lorem ipsum dolor'},
      {_id: 3, title:'Product 3', price:123.45, quantity: 10, description:'Lorem ipsum dolor'},
      {_id: 4, title:'Product 4', price:123.45, quantity: 10, description:'Lorem ipsum dolor'}
    ];
    
````

a seguir vamos injetar a fábrica em um controlador:

````js
angular.module('meanshopApp')
  .controller('ProductsCtrl',function ($scope, Product) {
    $scope.products = Product;
  });
  
````

podemos observar que apenas informamos o nome da fábrica e ela estará disponível para o controlador. e depois criamos a variável
$scope.products e com isso products estará disponível na view.

##### Criando um catálogo

Lembrando que estamos recebendo os dados de um factory que é injetada no controlador ProductsCtrl e disponibilizada através
$scope ( $scope.products ).

** LAYOUT clientes/app/products/products.html **
````html
<navbar></navbar>
<div class="container">
  <div class="row">
    <div class="col-sm-6 col-md-4">
      <h1>Products</h1>
      <p ng-show="products.length < 1"> No Products to show.</p>
      <a ui-sref="newProduct">New Product</a>
    </div>
  </div>
  <div class="row">
    <div class="col-sm-6 col-sm-4" ng-repeat="product in products">
      <div class="thumbnail">
        <img src="http://placehold.it/300x200" alt="{{product.title}}">
        <div class="caption">
          <h3>{{product.title}}</h3>
          <p>{{product.description | limitTo:100}}</p>
          <p>{{product.price | currency}}</p>
          <p>
            <a href="#" class="btn btn-primary" role="button">Buy</a>
            <a ui-sref="viewProduct({id:product._id})" class="btn btn-default" role="button">Details</a>
          </p>
        </div>
      </div>
    </div>
  </div>
</div>
<footer></footer>
````

<ul>
  <li><b>ng-include</b>: insere código a partir do HTML</li>
  <li><b>ng-repeat</b>: faz um loop em $scope.products criando uma div para cada produto.</li>
  <li><b>ui-sref</b>: chama um estado e passa os parâmetros para ele ex: /products/<b>:id</b></li>
</ul>

#### Filtros

Os filtros são uma ótima maneira para exibir um dado formatado acessando o DOM o uso do | ( pipe ) dentro de uma expressão
angular ( {{ }} ) podemos criar nossos próprios filtros e o angular disponibiliza alguns como:

<ul>
  <li><b>limitTo</b>: trunca a string ou array com um número especificado de caracteres</li>
  <li><b>currency</b>: faz o mesmo que number e adiciona um simbolo de moeda</li>
  <li><b>number</b>: esse filtro separa casas decimais</li>
  <li><b>json</b>: converte objetos javascript em string JSON</li>
  <li><b>lowercase/uppercase</b>: converte para caixa alta e baixa</li>
  <li><b>date</b>: pega o instante de tempo Unix e transforma em um formato especificado por parâmetros</li>
</ul>

#### Serviços

** cógido em client/app/products/products.service.js **

````js
'use strict';

angular.module('meanshopApp')
  .factory('Product', function () {

    var last_id = 5;

    var example_products = [
        {_id: 1, title:'Product 1', price:123.45, quantity: 10, description:'Lorem ipsum dolor'},
        {_id: 2, title:'Product 2', price:123.45, quantity: 10, description:'Lorem ipsum dolor'},
        {_id: 3, title:'Product 3', price:123.45, quantity: 10, description:'Lorem ipsum dolor'},
        {_id: 4, title:'Product 4', price:123.45, quantity: 10, description:'Lorem ipsum dolor'}
      ];

    return {

      query: function() {
        return example_products;
      },
      get: function(prod) {
        var result = {};
        angular.forEach(example_products,function(product) {
          if(product._id == prod.id)
            return this.product = product;
        } , result);
        return result.product;
      },
      create: function(product) {
        product._id = ++last_id;
        example_products.push(product);
      },
      delete: function(params) {
        angular.forEach(example_products,function(product, index) {
          if(product._id == params._id) {
            console.log(product,index);
            example_products.splice(index,1)
            return;
          }
        })
      },
      update: function(product) {
        var item = this.get(product);
        if(!item) return false;

        item.title = product.title;
        item.price = product.price;
        item.quantity = product.quantity;
        item.description = product.description;
        return true;
      }
    }
  });

````

Podemos ver que nada externo a fábrica tem acesso a examplo_products essa variável é privada enquanto que todos os métodos
devolvidos no objeto são públicos essa técnica é chamada de <b>closures</b>.

#### Controladores

<b>código em client/app/products/products.controller.js</b>

````js
'use strict';

angular.module('meanshopApp')
  .controller('ProductsCtrl', function ($scope, Product) {
    $scope.products = Product.query();
  })
  .controller('ProductViewCtrl',function($scope, $state, $stateParams, Product){
    $scope.product = Product.get({id:$stateParams.id});

    $scope.deleteProduct = function() {
      Product.delete($scope.product);
      $state.go('products');
    };
  })
  .controller('ProductNewCtrl',function($scope, $state, Product) {
    $scope.product = {};
    $scope.addProduct = function(product) {
      Product.create($scope.product);
      $state.go('products');
    };
  })
  .controller('ProductEditCtrl',function($scope, $state, $stateParams, Product){
    $scope.product = Product.get({id: $stateParams.id});

    $scope.editProduct = function(product) {
      Product.update($scope.product);
      $state.go('products');
    };
  });

````

adicionamos um controlador para cada opção de CRUD com exceção do delete que será chamado a partir de um botão na view
e por isso que o deleteProduct é mostrado dentro do controlador Product View, temos também uma injeção de dependência
com $scope, $state, $stateParams.
$state permite o redirecionamento para um estado ou rota diferente e $stateParams é um objeto que contém todas as variáveis
do URL.

#### Rotas

Depois de feito os controladores e serviços precisamos de uma rota que vincule um URL aos controladores e templates.

<b> client/app/products/products.js </b>
````js
'use strict';

angular.module('meanshopApp')
  .config(function ($stateProvider) {
    $stateProvider
      .state('products', {
        url: '/products',
        templateUrl: 'app/products/products.html',
        controller: 'ProductsCtrl'
      })

      .state('newProduct',{
        url:'/products/new',
        templateUrl:'app/products/templates/product-new.html',
        controller:'ProductNewCtrl'
      })

      .state('viewProduct', {
        url:'/products/:id',
        templateUrl:'app/products/templates/product-view.html',
        controller: 'ProductViewCtrl'
      })

      .state('editProduct', {
        url:'/products/:id/edit',
        templateUrl:'app/products/templates/product-edit.html',
        controller:'ProductEditCtrl'
      });
  });

````

#### Criação da view

os códigos da view vão estar em <b>client/app/products/templates/</b> vou falar sobre alguns detalhes aqui a criação das views
é algo que não vou explicar aqui, vou falar aqui sobre algumas diretivas do angular que são muito usadas nos templates para a
interação:

<ul>
  <li><b>ng-model</b>: vincula ao $scope ex: $scope.product.</li>
  <li><b>ng-include</b>: insere um arquivo html a partir de um caminho especificado.</li>
  <li><b>ng-submit</b>: disparado quando o formulário é enviado chamado o método addProduct em ProductNewCtrl.</li>
  <li><b>ui-sref</b>: chama um estado podendo passar parametros.</li>
  <li><b>ng-click</b>: dispara um ação a partir do click do botão.</li>
  <li><b>ng-show</b>: verifica uma condição e a partir da ai exibe ou não um elemento.</li>
</ul>

#RESUMO

uma introdução estamos usando dados disponíveis na memória que já nos dão uma idéia de como podemos substituir por requisição $jttp
a interação entre as camadas do angular me chamou muita atenção pois ainda não tinha criado um projeto seguinte tais pradrões
deixa o entendimento do código muito mais simples, o uso do router também foi uma ótima experiência.

