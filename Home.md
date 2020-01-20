# Welcome to the TensorMap wiki!

Tensormap is a web application that enables users to create deep learning models using a graphical interface without having to know how to code. We hope that this will help as a stepping stone to individuals who are starting to explore the deep learning paradigm.

### Listed below are the main functions of Tensormap

As a part of Google Summer of Code 2019 we implemented the following features:

* Create neural network architecture using drag and drop interface.
    * Create different nodes(input, hidden, output) by dropping to the workspace.
    * Link different nodes that define flow to data and model.
    * Group Various node to form a layer.
    * Define different parameters associated with nodes and layers.
    * Manually update and retrieve weight of links.
    * specify model compilation and execution configuration (like learning rate, optimizer, etc)
    * View Runtime results.
    * Get the generated code.
* Upload data of CSV format and visualize the uploaded data
* Perform data manipulations on uploaded data like:
    * Adding rows
    * Editing rows
    * Deleting rows
    * Deleting columns
    * Sorting columns
    * Filtering column data
    * Searching for data
* Specify dataset and experiment related configurations like:
    * Features
    * Labels
    * Test percentage
    * Experiment type (Binary Classification, Multiclass Classification, Regression)
    * Number of epochs
    * Batch size
    * Loss function (Binary Crossentropy, Categorical Crossentropy, Mean Squared Error )
    * Optimizer (Adam optimization)
* Download edited CSV
* Download the code that was auto-generated according to the created model
* Execute the created model and show the progress
* Show the resultant metric values
