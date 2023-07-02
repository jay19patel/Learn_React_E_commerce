# E Commerce Website

## => Pages
- HomePage
- AboutPage
- ProductPage
- ContactPage
- Login/RegistrationPage


## => Create ROuter for all url Pages
- install react-router-dom 
```js
// yarn add react-router-dom
import { BrowserRouter as Router,Routes,Route} from 'react-router-dom';  
// 
<Router>
      <NavigationBar/>
          <Routes>
            <Route path='/' Component={HomePage} exact ></Route>
            <Route path='/AboutPage' Component={AboutPage}></Route>
          </Routes>
        <Footer/>
      </Router>
```

## => Navigation Bar
- import all components after we use them in routing
```js
<nav>
    <Link to={'/'}><button>Home</button></Link>    
</nav>
// Also Use NavLink insted of Link
```

## => Create Some Pages using Bootstrap 5

## => Context API (IMP) Learn
- create a useContext => createContext
- provider
- consumer => useContext Hook

```js
// => ProductContext.jsx
import { createContext, useContext } from "react";
const AppContext = createContext();
const AppProvider = ({children})=>{
    return(
    <AppContext.Provider value={{myname:"Jay Patel"}}>
        {children}
    </AppContext.Provider>
    );
};

const useProductContext =()=>{
return useContext(AppContext);}
export {AppContext,AppProvider,useProductContext}

// => App.js

<AppProvider>...</AppProvider>
// wrap all Componenet wit <AppProvider> also  import them


// =>  use at Any Component Page
import { useProductContext } from '../Context/ProductContext'
const {myname}= useProductContext() // Recive data here  component na andar use karvu

```

## => Context API Fetch API (AXIOS)
- install axios (yarn add axios)
- API = "https://api.pujakaitem.com/api/products"

```js
//  Fetching API data uisng Axios 
const getProducts = async(url)=>{
        const response = await axios.get(url);
        const Products = await response.data;
        console.log(Products)
     }

//  Call function
useEffect(()=>{
        getProducts(API);
    },[])
    // if  [] not use then  useeffect run inifnity time 
```

## => State Managemnet (useReducer)
- => why use => alredy axios thi api to avi gay pan vare vare use karva ya filter karva ene call na karva pade atle apde ene ek jagiye stor kari des and pasi tya thi use karsu fillter kari kari ne 

```js
const [state ,dispatch] =useReducer(reducer,initialState)
//  State => Update Output Data
//  dispatch => Functionality add here
// reducer => functionality import from other file
// initialState => initialState of data
```
- main useReducer ma thay m ke e condication chake kare and jo conditionmatcch they to tema je code rey te paraman  initialState state na data ne chnage kare 

```js

dispatch({ type:"Set_Loading"});
dispatch({ type:"My_API_Data",payload:Products});
dispatch({ type:"My_Error"});

// also we use try and catch for error handling 

// => productsReducer.jsx

const ProductReducer = (state,action) => {
// initialState ={
    // isLoading:false,
    // isError:false,
    // products:[],
    // fearureProducts:[] }

//  check all action and action mujar case run thay and je fillter joye te paramne output mate code lakhvo
    switch(action.type){
        case "Set_Loading":
            return{
            ...state,
            isLoading:true  };

        case "Set_API_Data":
            const fearureData = action.payload.filter((e)=>{
                return e.fearured === true;
            })
            return {
                ...state,
                isLoading:false,
                products:action.payload,
                fearureProducts:fearureData

              };
        default:
            return state

    }
  return state
}
export default ProductReducer

```

## => Passing Data In URL
```js
 <Route path='/ProductDetailPage/:id' Component={ProductDetailPage}></Route> //App.js

 <Link to={`/ProductDetailPage/${id}`}><button >View</button></Link> //Where click function use also where id is available

//  acces id 
const {id} = useParams();
```



## =>Use Perems (get data from url) Product detail Page
- create api uisng reducer for single product
```js
//  Single Product Context => ProductContext.jsx
//  function pass is  AppProvider
    const singleProduct = async(url)=>{
        const response = await axios.get(url);
        const SingleProducts = await response.data;
        console.log("SingleProducts",SingleProducts)
        dispatch({type:"Single_Produsct",payload:SingleProducts})

    }
//  Get Single Product Fuction uisng Context API
let {singleProduct} = useProductContext()

// In Product Detail Page ,we use use Params for geting URL id 
// Use singleProduct function in useEffect Hook and pass url for single product
const {id} = useParams();

```
- chnage reducer and contect datas 
- pass the single data 

```js
// => ProductReducer.jsx
case "Single_Set_API_Data":
    return{ ...state,
            singleproductdata:action.payload  };
// Use any where you want
const {singleProduct, singleproductdata} = useProductContext()
```

























































## => Cart + or - System
- use state hook

```js
const [amount,setAmount]= useState(1);
//  create 2 function one increment or one for decrement 

function setIncrement(){amount>1?setAmount(amount+1):setAmount(1)}
function setDecrement(){amount>1?setAmount(amount-1):setAmount(1)}
// : => OR
// ? => Not
```

## => Reduce use to chnage component 
- use Reducer 
- context ni adar j reducer use karvu
- adding some variables in initialSate and chnage using Reducer 
- geting data with context api and chack varibale with if else ,if true then display that other wise else part code
- create Context  and  create some variables in initialSate
```js
const [state ,dispatch] =useReducer(reducer,initialState)
```
- create some function using dispatch
```js
dispatch({ type: "falanu",payload:Paylo_Data_1}); 
dispatch({ type: "dheknu",payload:Paylo_Data_2}); 
```

- in reducer file check falanu and dheknu and run code usring them
```js
const ProductReducer = (state,action) => {

    switch(action.type){
        case "falanu":
            return{
            message:"This Is Falanu" };
        case "dheknu":
            return{
            message:"This Is Dheknu"};
        // Both Case are false then run default case 
        default:
            return state
    }
}
export default ProductReducer
```
- in componet get message using context and check them using if and else if falanu them component1 else component2
'

## => Filter Product 
- use Reducer for filler data / contect API to geting Data 
- all fillter function create in contex file and call  from diffrent components 
- use simple logic and code in reducer file where trigger by contex dispatch()
- get each input/button name or value using dom event method 
```js
//  initialState => Default values 
const initialState = {
  filter_products: [],
  all_products: [],
  grid_view: true,
  sorting_value: "lowest",
  filters: {
    text: "",
    category: "all",
    company: "all",
    color: "all",
    maxPrice: 0,
    price: 0,
    minPrice: 0,
  },
};
// Contect page
const [state, dispatch] = useReducer(reducer, initialState); // call reducer


  // to sort the product
  useEffect(() => {
    dispatch({ type: "FILTER_PRODUCTS" });
    dispatch({ type: "SORTING_PRODUCTS" });
  }, [products, state.sorting_value, state.filters]);// geting data from context api


// Reducer page
    case "FILTER_PRODUCTS":
      let { all_products } = state;
      let tempFilterProduct = [...all_products];

      const { text, category, company, color, price } = state.filters;

      if (text) {
        tempFilterProduct = tempFilterProduct.filter((curElem) => {
          return curElem.name.toLowerCase().includes(text);
        });
      }

      if (category !== "all") {
        tempFilterProduct = tempFilterProduct.filter(
          (curElem) => curElem.category === category
        );
      }

      if (company !== "all") {
        tempFilterProduct = tempFilterProduct.filter(
          (curElem) => curElem.company.toLowerCase() === company.toLowerCase()
        );
      }

      if (color !== "all") {
        tempFilterProduct = tempFilterProduct.filter((curElem) =>
          curElem.colors.includes(color)
        );
      }

      return {
        ...state,
        filter_products: tempFilterProduct,
      };

    // Simple mean  all chnage set with initialState 
    case "CLEAR_FILTERS":
      return {
        ...state,
        filters: {
          ...state.filters,
          text: "",
          category: "all",
          company: "all",
          color: "all",
          maxPrice: 0,
          price: state.filters.maxPrice,
          minPrice: state.filters.maxPrice,
        },
      };



```

## => Adding Into Cart

-  add link with id to go cart page 
```js
<Link to={'/cart'} onClick=(()=>addToCart(id,amount,product))>Add To Cart</Link>  //add in product detail page 
//  ahiya click karsu to context ma addToCart run thase
// create addtoCart function
```
- when we click on btn then we need some datas of this product => id , total amount, contity,product details
- create cartContext page and cartReducer page

```js
// => Cart Contextpage
initialSate ={
    cart:[]
}
//  create cartContext page and create context
const [state, dispatch] = useReducer(reducer,initialSate)
const addToCart = (id, color, amount, product) => {
    dispatch({ type: "ADD_TO_CART", payload: { id, color, amount, product } });
  };
//  pass addToCart into Provider  jethi cart component m click karsu to ahiya run thase 

//  => reducer page
 if (action.type=="ADD_TO_CART"){
    let {id,color.amount,product} = action.payload;//via payload destructure

    let cartProduct = {
        name = "flanu dheknu dekhnu "// use product.name
        // get all useefull data here
    }
    return {
        ...state,
        cart:[...state.cart,cartProduct]// update cart into inisitalState cart (adding item in cart)
    }
 }
```



## => Detele Item
- when click on delete btn then run delete funtion 
- thet function call at context file 
- trigerting reducer then je id pass they e id sivay bija data display thay using => !== (filter function)


## => Loacl Storage for  save Cart Data 
```js
//   Use Loacal stoarge to save data in temporary

useEffect(()=>{
    localStorage.setItem("UserName_Cart",JSON.stringify(state.cart))
},[state.cart])
```
- jab jab cart chnage thase tab tab useEffect run thase and localStorage ma Update thatu rese 
```js
// Get LocalStorage Data
let newCartData= localStorage.getItem("UserName_Cart")
if(newCartData===[]){
    return []
}else{
    return JSON.parse(newCartData) ;
}
// in initialState chnage cart:[] => cart:functionName()
```
- if need to clear the cart then set => cart:[]

## => Total_Item 
- use Reducemethod 
```js
// jyare page cart change thay tab total chnage thay so use useEffect
useEffect(()=>{
    dispatch({type:"Cart_Total_Item"})
})

// => reduce file
if(action.type=="Cart_Total_Item"){
    let updateItemVal =state.cart.reduce((initial_Element,current_Element)=>{
        let {amount} = current_Element;
        initial_Element = initial_Element + amount
        return  initial_Element
    },0)

    return {
        ...state,
        total_item:updateItemVal // access and chnage initialSate Value
    }
}
```
- access total item at navbar uisng contextapi


## => Price Sub Total

- same as abov system (Total_Item )
- just add price in destructuring
```js
let {amount,price} = current_Element;
initial_Element = amount * price;
```
# Examples  
## Reducer 
```js
// Initial state
const initialState = {
  count: 0,
};

// Reducer function
const reducer = (state, action) => {
  switch (action.type) {
    case 'INCREMENT':
      return { ...state, count: state.count + 1 };
    case 'DECREMENT':
      return { ...state, count: state.count - 1 };
    case 'RESET':
      return { ...state, count: 0 };
    default:
      return state;
  }
};

```

```js
import React, { useReducer } from 'react';

const MyComponent = () => {
  const [state, dispatch] = useReducer(reducer, initialState);

  // Example usage
  const increment = () => {
    dispatch({ type: 'INCREMENT' });
  };

  const decrement = () => {
    dispatch({ type: 'DECREMENT' });
  };

  const reset = () => {
    dispatch({ type: 'RESET' });
  };

  return (
    <div>
      <p>Count: {state.count}</p>
      <button onClick={increment}>Increment</button>
      <button onClick={decrement}>Decrement</button>
      <button onClick={reset}>Reset</button>
    </div>
  );
};

```

## State 

```js
import React, { useState } from 'react';

const MyComponent = () => {
  // Initialize state using useState
  const [count, setCount] = useState(0);
  const [message, setMessage] = useState('');

  // Update state
  const increment = () => {
    setCount(count + 1);
  };

  const decrement = () => {
    setCount(count - 1);
  };

  const handleChange = (event) => {
    setMessage(event.target.value);
  };

  return (
    <div>
      <p>Count: {count}</p>
      <button onClick={increment}>Increment</button>
      <button onClick={decrement}>Decrement</button>

      <br />

      <input type="text" value={message} onChange={handleChange} />
      <p>Message: {message}</p>
    </div>
  );
};

export default MyComponent;


```




## UseEffect 
- data ma koy chnage ave tab te run thay
- bar thi ene chnage karva ni tye thay to run thY 

```js
useEffect(()=>{
    consol.log("run this code")

},[data])
//  if data are chnage then run print code 
``
- [data] lakhva thi data par nirbhar they jay jethi data ena juna data thi change ave to code run thay
- [] lakhvathi khali ek var code run thay ne pasi ruki jay

