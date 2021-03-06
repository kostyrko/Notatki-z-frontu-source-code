Title: React: formularze
Author: mkostyrko
Date: 2020-08-25 10:00
Updated:
Category: reactjs
Tags: react, formularze, walidacja, radio, select
Slug: react-formularze
related_posts: react-wprowadzenie, react-komponenty, react-listy

![react](https://vegibit.com/wp-content/uploads/2019/03/A-Simple-Reactjs-Form-Example.png){: max-height="300px"}

W kontekście Reacta można stosować dwa rodzaje formularzy - tzw. *kontorolowane* i *niekontorlowane*

Formularze *niekontorlowane* to są te, których logika nie jest sterowana przy pomocy JS/Reacta innymi słowy, których dane nie są przechwytywane przez stworzone komponenty aplikacji -> przeciwieństwem tego podejścia są **formularze kontrolowane**


### Stany ponad wszystko -> stan jako “wyłączne źródło prawdy”

Różnego rodzaju elementy formularzy takie jak `<input>` `<textarea>` `<select>` w HTML posiadają własny stan, który jest aktualizowany na podstawie informacji wpisywanych przez użytkownika w **recatowych formularzach kontrolowanych** ten stan jest przechwytywany i skorelowany ze stanem danego komponentu np. poprzez metodę setState lub inną przypisaną do zmiany stanu komponentu w przypadku komponentów funkcyjnych (stan może być zapisany jako obiekt i zbierać informację z wielu formularzy lub każdy z nich może mieć osobny stan).


Prosty przykład gdzie 1 funkcja obsługuje jeden element formularza


    class NameForm extends React.Component {
      constructor(props) {
        super(props);
        this.state = {value: ''};
      }

      handleChange = (event) => {    
        this.setState({value: event.target.value});
        console.log(this.state.value) // wartość stanu jest stale aktualizowana
      }

      render() {
        return (
          <form> 
            <input type="text" value={this.state.value} onChange={this.handleChange}/>
          </form>
        );
      }
    }


Przykład gdzie jedna funkcja obsługuje wiele elementów formularza


    class NameForm extends React.Component {
      constructor(props) {
        super(props);
        this.state = {
          name: '',
          surname: '';
          };
      }

      // funkcja wykorzystująca event i przypisująca dane do konkretnego elementu stanu
       o tej samej nazwie gdzie name === jak element, do którego wartość ma być przypisana
      handleChange = (event) => {    
        this.setState({
          [e.target.name]: event.target.value
        });
      }

      // funkcja zajmująca się przesłaniem danych
      handleSubmit(event) {
        alert('Podano następujące imię: ' + this.state.value);
        event.preventDefault();
      }

      render() {
        return (
          <form onSubmit={this.handleSubmit}>
            <input type="text" name='name' value={this.state.name} onChange={this.handleChange}/>
            <input type="text" name='surname' value={this.state.surname} onChange={this.handleChange}/>
            <input type="submit" value="Send"/>
          <form>
          
        );
      }
    }


W przypadku komponentów funkcyjnych możemy daną funkcję odpowiedzialną za zmianę stanu komponentu wpisać inlinowo


    const Form = () => {
      const [name, setName] = useState("");

    return (
      <form>
        <input type="text" value={name} onChange={e => setName(e.target.value)}/>
      </form>
      );
    };


W przypadku stworzenia jednego stanu dla wielu elementów formularza należy pamiętać o tym, że będzie musiał być on w całości przywołany - można do tego wkorzystać operatora rozpraszającego (destrukturyzacji)

    const Form = () => {
      const [form, setForm] = useState({
        name: '',
        surname: ''
      });

    const handleChange = (e) => {
      const {name, value} = e.target; // destrukturyzacja obiektu e.target na imię i wartość
      setForm(prevState => { // wykorzystanie poprzedniego stanu do jego modyfikacji
        return {    // zwrócenie obiektu
          ...prevState,
          [name]: value // wprowadzenie nowej wartości
          }
      });
    };

    return (
      <form>
        <input type="text" name='name' value={form.name} onChange={handleChange}/>
        <input type="text" name='surname' value={form.surname} onChange={handleChange}/>
      </form>
      );
    };


Custom hook dla formularza

Przykładowe użycie wygląda w następujący sposóbu
W komponencie wykorzystującym custom hook przy pomocy destrukturyzacji należy go zaimportować, użyć a następnie wyspredować lub wypisać


    // useInput.js
    import {useState} from "react";
    export default (valueOnStart) => {
      const [value, setValue] = useState(valueOnStart);
      
      return [
        value, // zwraca nazwę stanu i jest odpowiednikiem [name]: value 
        {   // tablica
          value,  // stan pola
          onChange: e => { // gdy wykryje zmianę aktualizuje stan
            setValue(e.target.value);
          }
        }
      ]
    };

    import React from "react";
    import useInput from "./hooks/useInput";

    const Form = () => {
      const [name, connectName] = useInput("");
      const [age, connectSurname] = useInput("");
      return (
        <form>
          // destrukturyzacja zwracanego obiektu przez custom hooka
          //, który jest tablicą {value, onChange: e=>{setValue(e.target.value)}}
          <input type="text" {...connectName}/> 
          <input type="number" {...connectSurname}/>
        </form>
        );
    };

### Formularze wielokrotnego wyboru (select -> option)

W pewnym sensie swoją własną specyfiką charakteryzuje się formularze wielokrotnego wyboru

W przypadku formularza wielokrotnego wyboru ten domyślnie jako wybraną opcję przyjmie pierwszą z możliwych - stąd jeśli będziemy chcieli ją wybrać zmiana nie zostanie odnotowana. W przypadku poniższego formularze nie możemy wybrać wartości *c3po* i ta staje się dostępna dopiero wówczas gdy zostanie wybrana jako kolejna wartość - jak temu zapobiec?

<p class="codepen" data-height="388" data-theme-id="light" data-default-tab="js,result" data-droid="mkostyrko" data-slug-hash="MWyvRVX" style="height: 388px; box-sizing: border-box; display: flex; align-items: center; justify-content: center; border: 2px solid; margin: 1em 0; padding: 1em;" data-pen-title="MWyvRVX">
  <span>See the Pen <a href="https://codepen.io/mkostyrko/pen/MWyvRVX">
  MWyvRVX</a> by Mikołaj Kostyrko (<a href="https://codepen.io/mkostyrko">@mkostyrko</a>)
  on <a href="https://codepen.io">CodePen</a>.</span>
</p>
<script async src="https://static.codepen.io/assets/embed/ei.js"></script>

**Rozwiązanie problemu**

**1** - Możemy w stanie przypisać wartość początkową, która później ulegnie zmianie i wskazuje użytkownikowi na wartość, która jest domyślną np *ms*

**2** - To rozwiązanie nie musi być jednak zawsze satysfakcjonujące stąd można jako pierwszą opcję wyboru dodać pozycję nie posiadającą wartości a "wypełnioną" tekstem np.

    <option value="">Select option</option>

Przykładowo

    import ReactDOM from "react-dom";
    import React, {useState} from "react";

    const Form = () => {
      const [droid, setDroid] = useState(''); // alternatywnie [droid, setDroid] = useState('c3po');  

      const handleDroidChange = (e) => {
        setDroid(e.target.value);
      };

      return (
        <form>
          <h1>Droid: {droid}</h1>

          <select value={droid} onChange={handleDroidChange}>
            // w alternatywnej wersji ta linia kodu nie jest potrzebna
            <option value="">Select option</option>
            <option value="c3po">C3PO</option>
            <option value="bb-8">BB-8</option>
            <option value="r2d2">R2D2</option>
          </select>
        </form>
      );
    };


### Radio input

W przypadku tzw inputu radiowego należy opcje wyboru zapakować w div lub inny element a następnie nasłuchiwać na zmiany jakie w nim zajdą / podobnie jak w przypadku inputu `select`


    <div onChange={e=>handleChange(e)}>
        <input type="radio" value="male" name="gender"/> Male
        <input type="radio" value="female" name="gender"/> Female
        <input type="radio" value="not given" name="gender"/> I don't wanna say
    </div>


:: **W jaki sposób sprawić aby przy pomocy Radio input istniała możliwość zaznaczenia tylko jednej opcji? Najłatwiej to to osiągnąć poprzez wskazanie wspólnej nazwy => name dla wszystkich inputów a przez to i stowrzenie ich wspólnej grupy (jak w powyższym przykładzie)**::


można również skorzystać z opcji stworzenia własnego hooka np.  `useRadioButtons`

źródło przykładu [StackOverflow-How to use radio buttons in ReactJS?](https://stackoverflow.com/questions/27784212/how-to-use-radio-buttons-in-reactjs)


    function useRadioButtons(name) {
      const [value, setState] = useState(null);

      const handleChange = e => {
        setState(e.target.value);
      };

      const inputProps = {
        name,
        type: "radio",
        onChange: handleChange
      };

      return [value, inputProps];
    }rgba(255, 255, 255, 0)

    użycie

    function App() {
      const [platformValue, plaftormInputProps] = useRadioButtons("platform");
      const [genderValue, genderInputProps] = useRadioButtons("gender");
      return (
        <div>
          <form>
            <fieldset>
            <fieldset>
              Male
              <input
                value="male"
                checked={genderValue === "male"}
                {...genderInputProps}
              />
              Female
              <input
                value="female"
                checked={genderValue === "female"}
                {...genderInputProps}
              />
            </fieldset>
          </form>
        </div>
      );


<iframe src="https://codesandbox.io/embed/6l6v9p0qkr?fontsize=14&hidenavigation=1&theme=dark"
     style="width:100%; height:500px; border:0; border-radius: 4px; overflow:hidden;"
     title="6l6v9p0qkr"
     allow="accelerometer; ambient-light-sensor; camera; encrypted-media; geolocation; gyroscope; hid; microphone; midi; payment; usb; vr; xr-spatial-tracking"
     sandbox="allow-forms allow-modals allow-popups allow-presentation allow-same-origin allow-scripts"
></iframe>


### Walidacja formularza

Walidacja może odbywać się na różne sposoby, ale jednym z prostszych a przez to ulubionych moich rozwiązań jest zastosowanie tablicy zbierającej informację o poszczególnych błędach, tą można przekazać do stanu a następnie wyrenderować

Przykładowo

    const validate = () => {
      const errorsArr = []
      if (!name || name.length < 5 && name.length >60 && !isNan(name) ) 
      {errorsArr.push('Name must be at least 2 characters and max of 60 and not a number')}
      if (!planet || planet.length < 5 && planet.length >60 && !isNan(planet) ) 
      {errorsArr.push('Planet must be at least 2 characters and max of 60 and not a number')}
      if (!age || age < 20) // input musi być type='number'
      {errorsArr.push('Droid should be at least 20 and must be a number')}
      if (errorsArr.length!==0) {
        setError(()=>errorsArr)
        return false
      } else {
        setError(null)
        return true
      }
    }


Taką funkcję walidującą można wykorzystać w innym miejscu danego komponentu np. w momencie przesyłania danych do serwera

      const [isSubmitted, setIsSubmitted] = useState(false);
      const [error, setError] = useState(null);

      const handleClick = (e) => {
        e.preventDefault(); // strona nie ma się odświeżać
        if(validate()) { // jeśli validate() true
          setIsSubmitted(true); // odpowiada za wyświetlania informacji o poprawnym zapisie
          fetch(API, {
            method: "POST",
            body: JSON.stringify(droid),
            headers: {
              "Content-Type": "application/json"
            }
            })
            .then(response => response.json())
            .then(droid => {
              // console.log(data);
              if(typeof addDroid==='function'){
                addUser(droid)
              }
            })
            .catch(error => {
              console.log(error);
              setError(error)
            });
            setTimeout(function(){ setIsSubmitted(false) }, 2000)
            // po upływie 2 sekund informacja znika 
        } else {
          setIsSubmitted(false);
        }
      }
  
      return (
        <>
          [...]
          // jeśli isSubmitted = true to wyświetl
          {isSubmitted && <h3>Data has been saved</h3>}

          // jeśli isSubmitted != null/false to wyświetl
          {error &&<ul>{error.map((error,i)=><li key={i}>{error}</li>)}</ul>}
        </>
      )


---

Więcej informacji na temat forms:

[formularze - React docs](https://pl.reactjs.org/docs/forms.html)

[A Simple React.js Form Example](https://vegibit.com/a-simple-react-js-form-example/)

[React form validation library built under 5kB](https://medium.com/@bruce1049/form-validation-with-hook-in-3kb-c5414edf7d64)

[GitHub: react-hook-form](https://github.com/react-hook-form/react-hook-form)

[YT- The BEST Way To Create Forms In React - React Hook Form Tutorial - How To Create Forms In React](https://www.youtube.com/watch?v=bU_eq8qyjic)

[StackOverflow-How to use radio buttons in ReactJS?](https://stackoverflow.com/questions/27784212/how-to-use-radio-buttons-in-reactjs)
