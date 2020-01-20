## Frontend
Technologies and strategies used to create the frontend:

### General
* **Programming languages:**
  *  Javascript, TypeScript *(used for workspace functionality)*
* **Technologies:**
  * [ReactJs](https://reactjs.org/)
  > Complete user interface is written in reactJs, because TensorMap has much dynamic data and components to handle.

  * [Material-ui](https://material-ui.com/)
  > Material UI components are used to make development fast and keep things minimalistic.

  * [Storm React Diagrams](https://github.com/projectstorm/react-diagrams)
  > Storm React Diagrams is a opensource project written in typescript to create network and flow graph in a reactjs application

### How it work
The main requirement of the project was the drag and drop functionality for nodes and links to sketch neural network architecture. [Storm React Diagrams](https://github.com/projectstorm/react-diagrams) has built in drag and drop function to create
 any component draggable and droppable workspace.{[Implementation details](https://github.com/projectstorm/react-diagrams/tree/master/packages/diagrams-demo-gallery/demos/demo-drag-and-drop)}<br>

At present **Nodes** are created using [DefaultNodeFactory](https://github.com/projectstorm/react-diagrams/blob/master/packages/react-diagrams-defaults/src/node/DefaultNodeFactory.tsx) which create simple one-way and two-way Nodes. Similarly Links use [DefaultLinkFactory](https://github.com/projectstorm/react-diagrams/tree/master/packages/react-diagrams-defaults/src/link).<br>
***Note:*** React diagrams provide **low-level** customization of these Components i.e we have to write own factories and models by extending the Default one.

There are different listeners attached to these nodes and links like **onDrop, onSelected, onDeleted, onDoubleClicked,** etc That handles the different callbacks and functions on frontend related to making call to backend and component state updation.

### Functions and Callbacks

* **Interact with Backend**<br>
> Function that request and put data to the Backend

1. [When Node is added](https://github.com/scorelab/TensorMap/blob/master/tensormap-client/src/pages/dashboard/scenes/a/components/assets/BodyWidget.tsx#L302-L321)
2. [When Node is Edited](https://github.com/scorelab/TensorMap/blob/master/tensormap-client/src/pages/dashboard/scenes/a/components/assets/BodyWidget.tsx#L323-L342)
3. [When Node is Deleted](https://github.com/scorelab/TensorMap/blob/master/tensormap-client/src/pages/dashboard/scenes/a/components/assets/BodyWidget.tsx#L284-L300)
4. [When Link created or deleted](https://github.com/scorelab/TensorMap/blob/master/tensormap-client/src/pages/dashboard/scenes/a/components/assets/BodyWidget.tsx#L121-L139)
5. [When link weight updated](https://github.com/scorelab/TensorMap/blob/master/tensormap-client/src/pages/dashboard/scenes/a/components/assets/BodyWidget.tsx#L122-L139)
6. [Call Training](https://github.com/scorelab/TensorMap/blob/master/tensormap-client/src/pages/dashboard/scenes/a/components/assets/BodyWidget.tsx#L220-L237)
7. [Get Code](https://github.com/scorelab/TensorMap/blob/master/tensormap-client/src/pages/dashboard/scenes/a/components/assets/BodyWidget.tsx#L239-L282)

<hr>

## Backend

Some important development notes:

### General
* The backend was implemented using Flask
* We have used MySQL as our database.
* SQLAlchemy was used to communicate with the database
* TensorFlow Keras was used for model code generation and execution
* The application as of now is only designed to be used by a single user.
* The backend of the project consists of several http routes and one socket-based route (for code execution).
* Mainly, there are 3 types of routes, they are:
  * code execution routes (dlmodel)
  * code auto-generation routes (template_manipulation)
  * pre-processing routes (data_preprocessing)

### Code auto-generation and execution

* As of now, Tensormap is only capable of auto-generating python code.
* We have used 3 template files to aid with code auto-generation, one for each experiment type: Binary Classification, Multiclass Classification and Regression
* The different metric types needed for each of these experiments were also implemented:
  * For classification: f1 score, precision, recall and accuracy
  * For regression: RMSE, MSE and MAE
* The model generation when executing the model is done by constructing a Keras compatible JSON using
This [Keras function](https://www.tensorflow.org/api_docs/python/tf/keras/models/model_from_json).
* This [link](https://docs.google.com/document/d/1Qyt8MBwkMPYhGnJpYs9e6QZ5QlLYrgfYfK4z0Catg_0/edit?usp=sharing) includes the JSON structures required by the code autogeneration routes. This will help in linking routes that are not properly linked to the frontend yet (eg: like experiment creation and deletion routes which should originally fire at user authentication). I have also included the limitations that were implemented. These limitations will be validated from the backend. JSON structures of other routes are not included as all of them have been already linked to the frontend.
* For every change in the frontend model creation workspace the relevant code generation route will be called to either create layer, edit layer, delete layer, edit experiment configurations (like the batch size, the number of epochs and etc).
* Layer creation is limited to dense layers as of now but can be extended to handling any type of layer.

### Pre-processing

* This element only accepts CSV data
* As of now, the pre-processing element in the project is capable of only handling regression-based CSV manipulations.

**NOTE:** You will notice 2 empty folders named "user_template" and "dataset" in the project structure, do not remove those folders as the user's code template and the uploaded data CSV will be saved in those respective folders.

### Database

* This consists of 4 tables:
  * code_layers: consists of all Keras code segments needed for code auto-generation and Keras model creation (from JSON).
  * template_copies: holds the current auto-generated code template.
  * user_template_index: holds the file line number of all the important code segments in the Keras template to aid in code auto-generation.
  * dataset: contains information relating to the uploaded dataset
