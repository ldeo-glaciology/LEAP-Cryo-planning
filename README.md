# LEAP-Cryo-planning

##  Introduction
A repo for planning and tracking progress on the LEAP-Cryo project: Learning ice-sheet flow with physics-based and machine learning models. This project is funded by the Learning Earth with Artificial Intelligence and Physics (LEAP) Science and Technology Center, based at Columbia University. 

We will track to do items and problems we encounter in the issues in this repo. Code development can occur either in this repop or in other repos created for specific subsets of the project. 

## Current project participants
Inital project participants (in alphabetical order):

Xianglong Bai, Columbia

Jonny Kingslake, LDEO/Columbia, @jkingslake

Andrew Hoffman, UW/LDEO/Columbia, @hoffmaao

Xuyuan Meng, Columbia, @novoland075

Hengbo Tong, Columbia, @Templar129

David Porter, LDEO/Columbia, @porterdf

Laure Zanna, NYU, @LaureZanna 


## Project description 

This descripion lays out the first steps of this project at a high level

Ice sheets are accumulations of ice 100's to 1000's of meter thick and 1000's of km across. They are so large that small changes in their size affects sea level, which affects coastal communities around the world. Ice sheets 'slide' over the sediments and rock beneath them. This is one main way that ice sheets shrink and cause sea-level rise, so it's very important to accurately describe sliding in models used to predict sea level.

This project is funded by Columbia's Learning the Earth with Artificial intelligence and Physics (LEAP) Center. Our overarching goal is to build and train machine learning models to accurately describe the mechanical stresses associated with ice-sheet sliding over bumpy sub-glacial topography (i.e. the ground beneath the ice sheet). The first step is to generate a training dataset using physics-based, high-resolution models of ice-sheet flow over bumpy topography. 
 
We will start by establishing a workflow for running an ensemble of ice-flow simulations using the open-source, python-based ice-flow solver, Icepack (https://icepack.github.io/). A key challenge will be designing the ensemble to effectively sample the relevant parameter space to allow efficient and comprehensive training of ML models on the results in the next stage of the project. This will likely include reducing the dimensionality of high-dimensional input fields, such as subglacial topography, to optimize the parameter-space sampling, and deciding how other parameters should be treated in the experimental design. 

With this workflow in place, we will aim to start running a large ensemble of forward simulations that will later be used for the training of ML models. 


