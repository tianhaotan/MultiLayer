# MultiLayer
<p>Transfer integral is a crucial parameter determines the charge mobility of organic semiconductors. The quantum chemical calculation of transfer integrals for all the molecular pairs in organic materials is usually an unaffordable task. Luckily this can be accelerated by the data-driven machine learning method. In this project, we develop machine learning models based on artificial neutral networks to predict transfer integrals accurately and efficiently. </p>
<p>This project includes script files necessary for generating features,  features filters, training models, and predicting transfer integrals for  organic semiconductor molecules quadruple thiophene with both dynamic and static disorders.</p>
<p><strong>########## Software/Libraries Requirement ##########</strong></p>
<ul>
<li>Python 3.8</li>
<li>Scikit Learn version 0.24.1</li>
<li>Numpy version 1.16.2</li>
<li>PyTorch version 1.8.0</li>
</ul>

<p>Folder contain scripts required for dataset generation, model training and transfer integral prediction for quadruple thiophene packing .</p>
<li>data0.txt #Contains the XYZ coordinates of each atom in a molecular pair extracted from MD simulations, with the unit in nm.</li>
<li>ce0 #Contains the cell vectors of the periodic box utilized in MD simulations, structured as XX YY ZZ XY XZ YX YZ ZX ZY, with the unit in nm.</li>
<li>id0 #A nearest neighbor list file with serial numbers starting from 1.</li>
<li>elelist #A PDB format file of a dimer. To ensure that the scripts accurately read the atomic order, a new structure must be generated in this sequential order.</li>
<li>import.txt #Contains the importance ranking obtained using the feature filtering method.  </li>
<li>make321mutil.py #A script for generating the overlap matrix, wherein the parameters for element specificity are already included. Note that overlap matrix elements relating to hydrogen atoms or intra-molecular terms will not be generated.</li>
<li>jefflod0.txt #Contains effective transfer integrals obtained via quantum chemical calculations and Lowdin’s orthogonalization. The units are in Hartree, and non-physical phases have not been corrected.</li>
<li>makefilter.py #Script for feature filtering</li>
<li>pred.py #Script for transfer integral prediction</li>
<li>finetune.py #Neural network training program for fine-tuning. Tfor fine-tuning. Upon fine-tuning, the resulting model will be saved as mlplossyGPB0&90.pth. This model can be accessed by using torch.load() </li>

<p><strong>########## Download Model and Dataset  ##########</strong></p>
The dataset, pre-trained model mlp.pth and fine-tuned model mlplossyGPB0&90.pth can be downloaded at https://mega.nz/folder/emZHGISL#ibR_KAmxVn2ngmwqzUXEaQ, and use

  ```
  torch.load(PATH)
  ```
to load the model

<p><strong>########## Prediction with Models ##########</strong></p>
<ol>
<li>Please perpare your own 3D coordinate files, lattice vector files, and nearest neighbor list files accordingly. Ensure that "makefilter.py," "make321mutil.py," "id0," "data0.txt," "ce0," and "import.txt" are all located within the same folder. It is important to maintain the atomic order and file format identical to those present in the project. You can verify this by opening the "elelist" file with a visualization program. </li>
</li>
<li>Modify make321mutil.py and makefilter.py  and run </li>
  <li>Run </li>

  ```
  ./make321mutil.py
  ```
<li>A feature file A321exx0.txt will be generated </li>
<li>Modify  makefilter.py  and run </li>

  ```
  ./makefilter.py
  ```
 
<li>Filtered feature file A321exx0_edit1.txt will be generated </li>
<li>Modify pred.py and run</li>
   
  ```
  ./pred.py
  ```
<li>Predicted transfer integral  will be generated in a file named 'predDIY'</li>
</ol>


<p><strong>########## Training yourself models ##########</strong></p>
<ol>
<li>Please ensure that you have downloaded the pre-trained model and dataset file. You can also read your own dataset file from the filtered feature file and transfer integrals file by run</li>



  ```
  X =open('A321exx0_edit1.txt','r')
  X = X.read()
  X = X.split()
  X = np.array(X).reshape((-1,3200))
  X = X.astype(float)
  Y =open('jefflod0.txt','r') 
  Y = Y.read()
  Y = Y.split()
  Y = np.array(Y)
  Y = Y.astype(float)
  np.savez('dataset.npz',X,Y)
  ```


<li>Modify the python interpreter path to your own dataset path and pre-trained model path </li>
<li>Run</li>
</ol>
  
  ```
  ./finetune.py
  ```
  During the training process, the model's performance will be displayed on the screen.
  And a model file named "mlplossyGPB0&90.pth" will be created.
</li>

