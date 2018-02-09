# React Router Demo

## Empezando

Corre `yarn install` y `yarn start`. No tenemos mucho solo un componente App.

### Demo 1

Vas a tener que hacer las siguientes tareas en el Primer Demo:

Configurar el Router:

```JSX
import React from 'react';
import ReactDOM from 'react-dom';
import { BrowserRouter } from 'react-router-dom';
/* App es el punto de entrada del código de React. */
import App from './App';

ReactDOM.render(
  <BrowserRouter>
    <App />
  </BrowserRouter>
  , document.getElementById('root'));
```

  Luego agregar rutas y links de esta forma:

```JSX
import React from 'react';
import { Link, Route } from 'react-router-dom';

/* Home component */
const Home = () => (
  <div>
    <h2>Home</h2>
  </div>
);

/* Category component */
const Category = () => (
  <div>
    <h2>Category</h2>
  </div>
);

/* Products component */
const Products = () => (
  <div>
    <h2>Products</h2>
  </div>
);

/* App component */
const App = () => (
  <div>
    <nav>
      <ul>

        {/* Componentes Links son usados para linkear a otras vistas */}
        <li><Link to="/">Homes</Link></li>
        <li><Link to="/category">Category</Link></li>
        <li><Link to="/products">Products</Link></li>
      </ul>
    </nav>

    {/* Componentes Route son rendereizados si el prop `path` coincide con el URL actual */}
    <Route path="/" component={Home} />
    <Route path="/category" component={Category} />
    <Route path="/products" component={Products} />
  </div>
);

export default App;
```

Luego solucionar el bug usando el prop `exact` en home


### Demo 2

Agregar una ruta con un parametro para mostrar el switch:

```JSX
<Switch>
    <Route exact path="/" component={Home} />
    <Route path="/products" component={Products} />
    <Route path="/category" component={Category} />
    <Route 
      path="/:id" 
      render={() => (
        <p>
          Quiero que este texto aparezca para todas las rutas excepto '/', '/products' y '/category'
        </p>
      )} 
    />
</Switch>
```

Cambiar el componente category por este (escrito a mano):

```JSX
import React from 'react';
import { Link, Route } from 'react-router-dom';

const Category = ({ match }) => (
  <div> 
    <ul>
      <li><Link to={`${match.url}/shoes`}>Shoes</Link></li>
      <li><Link to={`${match.url}/boots`}>Boots</Link></li>
      <li><Link to={`${match.url}/footwear`}>Footwear</Link></li>
    </ul>
    <Route
      path={`${match.path}/:name`} 
      render={({ match }) => (<div><h3>{match.params.name}</h3></div>)}
    />
  </div>
);

export default Category;
```

Usar estos datos para los productos:

```js
const productsData = [
{
  id: 1,
  name: 'NIKE Liteforce Blue Sneakers',
  description: 'Lorem ipsum dolor sit amet, consectetur adipiscing elit. Proin molestie.',
  status: 'Available'

},
{
  id: 2,
  name: 'Stylised Flip Flops and Slippers',
  description: 'Mauris finibus, massa eu tempor volutpat, magna dolor euismod dolor.',
  status: 'Out of Stock'

},
{
  id: 3,
  name: 'ADIDAS Adispree Running Shoes',
  description: 'Maecenas condimentum porttitor auctor. Maecenas viverra fringilla felis, eu pretium.',
  status: 'Available'
},
{
  id: 4,
  name: 'ADIDAS Mid Sneakers',
  description: 'Ut hendrerit venenatis lacus, vel lacinia ipsum fermentum vel. Cras.',
  status: 'Out of Stock'
},

];
```

Usar para mostrar rutas con parametro

```JSX
/* Import statements fueron omitidos para ser breves  */

const Products = ({ match }) => {
   const productsData = [
    {
        id: 1,
        name: 'NIKE Liteforce Blue Sneakers',
        description: 'Lorem ipsum dolor sit amet, consectetur adipiscing elit. Proin molestie.',
        status: 'Available'

    },
    // El resto de los productos fueron omitidos para ser breves
];


  /* Crea un arreglo de `<li>` por cada producto */
  const linkList = productsData.map(product => (
    <li key={product.id}>
      <Link to={`${match.url}/${product.id}`}>
        {product.name}
      </Link>
    </li>
  ));

  return (
    <div>
      <div>
        <div>
          <h3> Products</h3>
          <ul> {linkList} </ul>
        </div>
      </div>

      <Route
        path={`${match.url}/:productId`}
        render={props => <Product data={productsData} {...props} />}
      />
      <Route
        exact
        path={match.url}
        render={() => (
          <div>Por favor seleccione un Producto.</div>
        )}
      />
    </div>
  );
};

export default Products;
```

Crear el componente Product:

```JSx
const Product = ({ match, data }) => {
  const product = data.find(p => p.id == match.params.productId);
  const productData = product ? (
    <div>
      <h3> {product.name} </h3>
      <p>{product.description}</p>
      <hr />
      <h4>{product.status}</h4>
    </div>
  )
    :
    <h2>Perdón. El producto no existe </h2>;

  return (
    <div>
      <div>
        {productData}
      </div>
    </div>
  );
};

export default Product;
```


