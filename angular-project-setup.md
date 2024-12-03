# Angular Project Setup

This guide was made using
- [Node.js LTS](https://nodejs.org/en) version 22.3.0
- [Angular CLI](https://github.com/angular/angular-cli) version 18.0.4

Good example Angular project: https://github.com/gothinkster/angular-realworld-example-app



## Node Setup

1. Download and install [Node.js LTS](https://nodejs.org/en/download/prebuilt-installer) if you don't have it already.

2. Run the following in a Terminal to install pnpm if you don't have it already.

   ```cmd
   npm install -g pnpm
   pnpm setup
   ```

   - You will have to logout and back into Windows after the installation is complete.

3. Run the following in a Terminal to install the Angular cli if you don't have it already.
   ```cmd
   pnpm add -g @angular/cli
   ```



## Project Setup

1. Run the following in a Terminal in your workspace

   ```cmd
   ng new project-name --style=scss --routing --skip-tests --ssr=false --package-manager=pnpm
   ```
   
2. Replace the code in `project-name\src\app\app.component.html` with this

    ```html
    <router-outlet></router-outlet>
    ```

3. Open the `project-name\src\index.html` and set the title tag to the title of your app

4. Open the `project-name\src\app\app.component.ts` and set the title variable to the title of your app

5. Open the `project-name\package.json` and add this into the scripts list
    ```json
    "build:prod": "ng build --configuration production",
    "serve": "ng serve"
    ```

6. Replace the `project-name\src\favicon.ico` with your own icon

7. Open the `tsconfig.json` and add this in the compilerOptions
    ```json
    ,"paths": {
      "src/*": ["./src/*"]
    }
    ```



## How to serve a project
This lets you view the webpage with live updates from the project
1. Open a vscode terminal in the projects folder and run this command
    ```cmd
    pnpm serve
    ```

2. Go to [http://localhost:4200](http://localhost:4200) to see the served content



## How to build a project
This builds the html, css and js files that are used for the website
1. Open a vscode terminal in the projects folder and run this command
	```cmd
	pnpm build
	```

2. The ouput will be saved to `project-name\dist`



## How to create a new component (An HTML page)
Components consists of an HTML file, a TypeScript file and a SCSS (CSS) file. Components can be used as part of another component or as there own web page
1. Open a vscode terminal in the projects folder and run this command but replace component-name with the name of the component you want to create
	```cmd
	ng generate component component-name
	(or)
	ng g c component-name
	```



## How to create a route to a component
A route links a path from a url to a component
1. Open the `project-name\src\app\app-routing.module.ts` and add this code into the routes array. Replace routes-path with the path you want or empty for no path. Replace NameOfComponent with the name of the class in the components ts file
	```ts
	{ path: 'routes-path', component: NameOfComponent },
	```



## How to create a route to redirect to another route
This allows you to redirect one path to another
1. Open the `project-name\src\app\app-routing.module.ts` and add this code into the routes array. Replace routes-path with the path you want or empty for no path. Replace redirect-path with the path you want it to redirect
	```ts
	{ path: 'routes-path', redirectTo: 'redirect-path', pathMatch: 'full' },
	```



## How to create a new service
A service is TypeScript code that can be used across multiple components
1. Open a vscode terminal in the projects folder and run this command. Replace service-name with the name of the service you want to create
	```cmd
	ng generate service service-name
	(or)
	ng g s service-name
	```



## Adding assets
Adding assets like images used for the website
1. Add the asset to the `project-name\src\assets` folder or a folder in it.

2. You then get the image with the src `./assets/the-file-you-want.png`



## Sending data from parent to child component

1. In the child components ts file add this inside the component class. Replace nameOfValue with the name you want the value to be. Replace typeOfValue with the type of the value you want it to be
	```ts
	@Input() nameOfValue: typeOfValue;
	```

2. In the parent components html file add this to the inside of the child components tag. Replace nameOfValue with the name of the value you are setting. Replace value with the value you are setting
	```html
	nameOfValue="value"
	```
or this to link it to a variable in the parent components ts file. Replace nameOfValue with the name of the value you are setting. Replace variable with the name of the variable in the parent components ts file
	```html
	[nameOfValue]="variable"
	```



## Sending data from child to parent component

1. In the child components ts file add this inside the component class. Replace nameOfEvent with the name you want the event to be. Replace typeOfValue with the type of the value you want it to be
	```ts
	@Output() nameOfEvent = new EventEmitter<typeOfValue>();
	```

2. In the parent components html file add this to the inside of the child components tag. Replace nameOfEvent with the name of the event you are setting. Replace functionInParent with a function in the parent components ts file where $event is the value that was passed to the event
	```html
	(nameOfEvent)="functionInParent($event)"
	```

3. To send data to the event run this line in the child components ts file. Replace nameOfEvent with the name of the event you are calling. Replace valueToSend with the value you are sending to the event
	```ts
	this.nameOfEvent.emit(valueToSend);
	```



## Call a child components function from a parent component

1. In the parent components ts file add this inside the component class. Replace functionName with the name of the function. Replace returnValue with the the value that you are sending over
	```ts
	functionNameSubject: Subject<returnValue> = new Subject<returnValue>();
	```

2. In the child components ts file add this inside the component class. Replace functionName with the name of the function. Replace returnValue with the the value that you are sending over
	```ts
	@Input() functionNameSubject: Subject<returnValue> | undefined;
	```

3. In the parent components html file add this to the inside of the child components tag. Replace functionName with the name of the function
	```html
	[functionNameSubject]="functionNameSubject"
	```

4. In the child components ts file subscribe to the functionNameSubject like this
	```ts
	this.functionNameSubject?.subscribe({
	  next: (value: returnValue) => {
		// Do things
	  },
	  error: (error: any) => {
		console.error(error);
	  },
	});
	```

5. To call the function with the data do run this line in the parent ts file
    ```ts
    this.functionNameSubject.next(value);
    ```



## Adding Ideal Supply Angular Material (TODO: Update to new)
Angular Material is used to format the webpage
1. Open a vscode terminal in the projects folder and run this command. Saying Y to `Would you like to proceed?`. Selecting Custom for `Choose a prebuilt theme name, or "custom" for a custom theme`. Say Y for `Set up global Angular Material typography styles?`. Say Y for `Set up browser animations for Angular Material?`
    ```cmd
    ng add @angular/material
    ```

2. Create a file called `.npmrc` in the projects folder and set the text to this if you have not already
    ```
    registry=http://npm.conceptual.ca
    //npm.conceptual.ca/:_authToken="9wdhhG8fctdY6fOwFNX5vlLczmXrqOTuKOO875HuqRQ="
    //npm.pkg.github.com/:_authToken=${GITHUB_TOKEN}
    @nfc-authority:registry=http://npm.conceptual.ca
    @idealsupply:registry=http://npm.conceptual.ca
    @cpangular:registry=https://npm.pkg.github.com
    ```

3. Open a vscode terminal in the projects folder and run this command
    ```cmd
    yarn add @idealsupply/nglib-idealsupply-theme @cpangular/material-dynamic-theming change-case
    ```

4. Replace the code of `project-name\src\styles.scss` with this
    ```scss
    @use "@idealsupply/nglib-idealsupply-theme" as theme;
    @include theme.material-styles();
    ```

5. Add this to the top of `project-name\src\app\app.module.ts`. Feel free to remove anything you dont use
    ```ts
    // Material Form Controls
    import { MatAutocompleteModule } from '@angular/material/autocomplete';
    import { MatCheckboxModule } from '@angular/material/checkbox';
    import { MatDatepickerModule } from '@angular/material/datepicker';
    import { MatFormFieldModule } from '@angular/material/form-field';
    import { MatInputModule } from '@angular/material/input';
    import { MatRadioModule } from '@angular/material/radio';
    import { MatSelectModule } from '@angular/material/select';
    import { MatSliderModule } from '@angular/material/slider';
    import { MatSlideToggleModule } from '@angular/material/slide-toggle';
    // Material Navigation
    import { MatMenuModule } from '@angular/material/menu';
    import { MatSidenavModule } from '@angular/material/sidenav';
    import { MatToolbarModule } from '@angular/material/toolbar';
    // Material Layout
    import { MatCardModule } from '@angular/material/card';
    import { MatDividerModule } from '@angular/material/divider';
    import { MatExpansionModule } from '@angular/material/expansion';
    import { MatGridListModule } from '@angular/material/grid-list';
    import { MatListModule } from '@angular/material/list';
    import { MatStepperModule } from '@angular/material/stepper';
    import { MatTabsModule } from '@angular/material/tabs';
    import { MatTreeModule } from '@angular/material/tree';
    // Material Buttons & Indicators
    import { MatButtonModule } from '@angular/material/button';
    import { MatButtonToggleModule } from '@angular/material/button-toggle';
    import { MatBadgeModule } from '@angular/material/badge';
    import { MatChipsModule } from '@angular/material/chips';
    import { MatIconModule } from '@angular/material/icon';
    import { MatProgressSpinnerModule } from '@angular/material/progress-spinner';
    import { MatProgressBarModule } from '@angular/material/progress-bar';
    import { MatRippleModule } from '@angular/material/core';
    // Material Popups & Modals
    import { MatBottomSheetModule } from '@angular/material/bottom-sheet';
    import { MatDialogModule } from '@angular/material/dialog';
    import { MatSnackBarModule } from '@angular/material/snack-bar';
    import { MatTooltipModule } from '@angular/material/tooltip';
    // Material Data tables
    import { MatPaginatorModule } from '@angular/material/paginator';
    import { MatSortModule } from '@angular/material/sort';
    import { MatTableModule } from '@angular/material/table';
    // Angular Form Controls
    import { FormsModule, ReactiveFormsModule } from '@angular/forms';
    ```

6. Add this to the top of the imports list in `project-name\src\app\app.module.ts`. Feel free to remove anything you dont use
    ```ts
    // Material Form Controls
    MatAutocompleteModule,
    MatCheckboxModule,
    MatDatepickerModule,
    MatFormFieldModule,
    MatInputModule,
    MatRadioModule,
    MatSelectModule,
    MatSliderModule,
    MatSlideToggleModule,
    // Material Navigation
    MatMenuModule,
    MatSidenavModule,
    MatToolbarModule,
    // Material Layout
    MatCardModule,
    MatDividerModule,
    MatExpansionModule,
    MatGridListModule,
    MatListModule,
    MatStepperModule,
    MatTabsModule,
    MatTreeModule,
    // Material Buttons & Indicators
    MatButtonModule,
    MatButtonToggleModule,
    MatBadgeModule,
    MatChipsModule,
    MatIconModule,
    MatProgressSpinnerModule,
    MatProgressBarModule,
    MatRippleModule,
    // Material Popups & Modals
    MatBottomSheetModule,
    MatDialogModule,
    MatSnackBarModule,
    MatTooltipModule,
    // Material Data tables
    MatPaginatorModule,
    MatSortModule,
    MatTableModule,
    // Angular Form Controls
    FormsModule,
    ReactiveFormsModule,
    ```



## Using an Ideal Supply webservice (TODO: Make sure it works on new angular version)
Used to add a webservice that calls one of our api endpoints. When the angular project is served localy it will use the localy run webservice, so make sure it is running when you use it. When you put it to production it will use the api-dev endpoint
1. Create a file called `.npmrc` in the projects folder and set the text to this if you have not already
    ```
    registry=http://npm.conceptual.ca
    //npm.conceptual.ca/:_authToken="9wdhhG8fctdY6fOwFNX5vlLczmXrqOTuKOO875HuqRQ="
    //npm.pkg.github.com/:_authToken=${GITHUB_TOKEN}
    @nfc-authority:registry=http://npm.conceptual.caHow to create a new service
    @idealsupply:registry=http://npm.conceptual.ca
    @cpangular:registry=https://npm.pkg.github.com
    ```

2. Open a vscode terminal in the projects folder and run this command. Replace webservice-name with the name of the webservice you want
    ```cmd
    yarn add @idealsupply/ngclient-webservice-webservice-name
    ```

3. Open `project-name\src\environments\environment.ts` and add this to the bottom of the environment object if you have not already
    ```ts
    ,appProviders: [],
    ```

4. Open `project-name\src\environments\environment.ts` and add this top of the file for each webservice you are going to use. Replace WEBSERVICE-NAME with the name of the webservice in all caps. Replace webservice-name with the name of the webservice.
    ```ts
    import { BASE_PATH as WEBSERVICE-NAME_BASE_PATH } from '@idealsupply/ngclient-webservice-webservice-name';
    ```

5. Open `project-name\src\environments\environment.ts` and add this into the appProviders in the environment object for each webservice you are going to use. Replace WEBSERVICE-NAME with the name of the webservice in all caps. Replace webservice-port with the port the webservice runs on. Replace webservice-name with the name of the webservice. 
    ```ts
    {
    provide: WEBSERVICE-NAME_BASE_PATH,
    useValue: 'http://localhost:webservice-port/webservice-name/v0',
    },
    ```

6. Open `project-name\src\environments\environment.prod.ts` and add this to the bottom of the environment object if you have not already
    ```ts
    ,appProviders: [],
    ```

7. Open `project-name\src\environments\environment.prod.ts` and add this top of the file for each webservice you are going to use. Replace WEBSERVICE-NAME with the name of the webservice in all caps. Replace webservice-name with the name of the webservice.
    ```ts
    import { BASE_PATH as WEBSERVICE-NAME_BASE_PATH } from '@idealsupply/ngclient-webservice-webservice-name';
    ```

8. Open `project-name\src\environments\environment.prod.ts` and add this into the appProviders in the environment object for each webservice you are going to use. Replace WEBSERVICE-NAME with the name of the webservice in all caps. Replace webservice-name with the name of the webservice. 
    ```ts
    {
    provide: WEBSERVICE-NAME_BASE_PATH,
    useValue: 'https://api-dev.idealsupply.com/webservice-name/v0',
    },
    ```

9. Open `project-name\src\app\app.module.ts` and put this at the top of the file if you have not already
    ```ts
    import { environment } from 'src/environments/environment';
    ```

10. Open `project-name\src\app\app.module.ts` and add this to the providers list if you have not already
    ```ts
    environment.appProviders
    ```

11. Open `project-name\src\app\app.module.ts` and add this to the top of the file if you have not already
    ```ts
    import { HttpClientModule } from '@angular/common/http';
    ```

11. Open `project-name\src\app\app.module.ts` and add this to the imports list if you have not already
    ```ts
    HttpClientModule,
    ```



<br>

## Common Errors

### error: Invalid version: "15.2-15.3"
Open the `project-name\.browserslistrc` and add these lines to the bottom
```
not ios_saf 15.2-15.3
not safari 15.2-15.3
```

<br>
