1. Download SOM_TOOLBOX from: http://www.cis.hut.fi/projects/somtoolbox/

2. Unzip the 'somtoolbox2_Mar_17_2005.zip' file into 'C:\MATLAB\toolbox\' folder.

3. Open MATLAB.

4. Set the PATH to newly created folder ('C:\MATLAB\toolbox\somtoolbox\') 
using MATLAB menu: File / Set Path / Add Folder /
Then select folder: 'C:\MATLAB\toolbox\somtoolbox\'

5. Make the subfolder 'C:\MATLAB\work\CPDIR'
Store the data from 'som_counter_prop_demo.zip' 
into the 'C:\MATLAB\work\CPDIR' subfolder.

6. Move to the subfolder 'C:\MATLAB\work\CPDIR'

7. Run the 'SOM_COUNTER_PROP_DEMO.m'

8. To run the som_counter_prop program ouside the DEMO, please use the following sintax:
> [rmsec, rmsep, sM, tren_pred, test_pred] = ...
         som_counter_prop(sD, sDtest, width, length, rough, fine, mask, init, lattice, shape, neighf, learnf, labels);

9. After the training is finished, use the SOM_TOOLBOX visualization tools.

10. Enjoy the new tool!

11. DO NOT FORGET to cite our program in your work: 
Igor Kuzmanovski, Marjana Novic, COUNTER-PROPAGATION NEURAL NETWORKS IN MATLAB, Chemometrics and Intelligent Laboratory Systems 90, 84–91 (2008).