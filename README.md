<b>Summary:</b> This repo contains the exercises and useful notes taken from the React Course taught by Maximilian Schwarzmuller. 

### Table of contents

|folder name               | Content description |
|--------------------------|:--------------------|
| section-3  | This folder contains the basics about setting up your first react app and useful notes to understand the basics of props and state. The things I learened in this chapter were the use of state and how to change it, the props functionality which is used to pass content through components. |
| section-4  | This folder contains the same app of section-3 but we added new features such as two new event listeners, for instance, when we click on the button show persons then the cards of the people are displayed and every time we click on a person card then that card gets deleted. The things I learned in this chapter were lists and if conditionals.  |                               |
| section-5             | This folder contains same project from section-4 but we added some css functionalities with Radium.|
| section-5-css-enabled | This folder contains same project from section-5 but the css functionalities were implemented with CSS modules. Furthermore, we enabled the ErrorBoundary component which is a useful tool to use in production because you have a way to control or catch an error that you believe might appear in another component. |
| section-6             | This folder contains the same project from section-5 with a different file structure.|
| Assignments           | See the completed assignments.|

### Intro
How do you start a react web app?

You need to first install nodejs in your machine, then that will install the npm package manager and after this simply type in a terminal window the command `npm init react-app <name-of-your-app>`.

What is the difference between package.json and package-json.lock? Package.json contains the dependencies and their current versions that your app needs in order to be built. The package.json.lock file contains the number of version of the dependencies that your dependencies need in order to build your app (nested dependencies). 

In order to install the dependencies from the `package.json` you need to run `npm install` in the folder where your `package.json` file is located. 

In order to install the nested dependencies exact version numbers from the `package-lock.json` you need to run `npm ci` in the folder where your `package-lock.json` file is located. Notice that you need both the package-lock.json and 


### section-3

First you need to execute `npm install` to download and install all the required dependencies from the package.json file and then you can run your react app by executing `npm start`. 

Remember how this app works.

     -----------        ---------         -----------
    | index.js | ----> |  App.js | ----> | Person.js |
     -----------        ---------         -----------

#### Render process

index.js renders the content of the file App.js with `ReactDOM.render(<App />, document.getElementById('root'));`. App.js has a Person component which used to render the `<Person>` tags. 

#### state and event handlers

App.js contains a state for the 3 `<Person>` tags and 2 event handlers that listen to the users' actions (the nameChangeHandler is not used).

A <b>state</b> is a property only available to the class components and what it does is to hold a state that changes inside of the class as in contrast with <b>props</b> which changes from outside - more information will be covered later on; furthermore, you can manipulate the state property with the `<b>this</b>` keyword. It is not recommended to use it too heavily because applications might become unpredictable. 

    // states being hold

    state = {

        people: [{name: 'Alex', age: 28},

        {name: 'Liz', age: 29},

        {name: 'Stephanie', age: 30}

        ]
    }

    // event handler
    
    switchNameHandler = (newName) => {
    
    this.setState({
          
          people: [{name: newName, age: 22},
                  
                  {name: 'Liz', age: 24},

                  {name: 'Stephanie', age: 32}
                
                ]})
    }

When the user clicks on the button the switchNameHandler event listener goes into its definition and sets the new state for the class component. 

    <button  style={style}
        onClick={() => this.switchNameHandler('Alejandro B.')}>Switch name</button>


### section-4

The render process for the app was changed and we display a button which shows or hides the people's cards. 

    return (
            <div className="App">
                <h1>React App</h1>
                <p>This is working!</p>
                <button  style={style}
                    onClick={this.togglePersonsHandler}>Switch name</button>  
                    {persons}
            </div>       
            );

The state of the App contains a new variable which is showPersons that will be used to show the contents of the App.  

class App extends Component {
  state = {
      people: [{id: 'a123', name: 'Alex', age: 28},
               {id: 'b123', name: 'Liz', age: 29},
               {id: 'c123', name: 'Stephanie', age: 30}
              ],
              // showPersons is used to display the cards of the other people with the if conditional
              showPersons: false
          }

When a user clicks on the Switch name button then the function togglePersonsHandler gets a copy of showPersons state then I have to map each person that is located in the array of people and display its contents. 

    togglePersonsHandler = () => {
        const doesShow = this.state.showPersons;
        this.setState({showPersons: !doesShow});
    }

    if(this.state.showPersons){
      persons = (
        // persons contain the mapping of each person of the array called people
        // key is used in react to keep track of all the elements id or indexes
          <div>
            {this.state.people.map((person, index) => {
              return <Person 
                      click={() => this.deletePersonHandler(index) }
                      name={person.name} 
                      age={person.age} 
                      key={person.id}
                      changed={(event) => this.nameChangeHandler(event, person.id)}
                    />
            })}
          </div>
        );
      }

Notice that each person has a click listener which basically indicates to delete that person card when the user clicks on it. 

    deletePersonHandler = (personIndex) => {
        const persons = [...this.state.people];
        persons.splice(personIndex, 1);
        this.setState({people: persons});
    }

Lastly, when you type in the input element of each person's card you activate the function nameChangedHandler that allows to change each name that was initially stored in the array. 

    nameChangeHandler = (event, id) => {
        // get the value of the person you want to change by its id
        const personIndex = this.state.people.findIndex(p => {
        return p.id === id;
        });

        // make a copy of the index of the personIndex
        const person = {
        ...this.state.people[personIndex]
        }

        // change the name of the person to the new value
        person.name = event.target.value;

        //make a copy of the people objects
        const copyPeople = [...this.state.people];
    
        //change the value of the copied object people
        copyPeople[personIndex] = person;

        //update the state of the old object people to the new state
        this.setState({people: copyPeople});
    }

This is how the structure of the react app works:

     
    App.js ---> waits for an event ---> click on button ---> togglePersonsHandler ---> display <Person /> 
    

### section-5-CSS-enabled

We had to run the `npm run eject` to dive into the internals of webpack (this command can only be executed when you have committed all the changes in git, otherwise it won't let you proceed). 
