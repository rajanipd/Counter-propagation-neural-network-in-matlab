# Counter-propagation-neural-network-in-matlab
Application of Counter Propagation neural network to predict customer churn and non churn in Telecommunication Industry
%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%
% SOM_COUNTER_PROP_DEMO - conter-propagation neural networks demo script
%
% Igor Kuzmanovski(*) and Marjana Novic(**)
%
% * Institute of Chemistry,
%   Arhimedova 5,
%   1001 Skopje, Macedonia
%   shigor@iunona.pmf.ukim.edu.mk
%
% **National Institute of Chemistry,
%   Hajdrihova 19,
%   1115 Ljubljana, Slovenia
%   marjana.novic@ki.si
% 
% REFERENCES
% R. Hecht-Nielsen, Proc. IEEE First Int. Conf. on Neural Networks, 1997 (II), pp. 19.
% R. Hecht-Nielsen, Appl. Optics, 26 (1987) 4979.
% R. Hecht-Nielsen, Neural Networks, 1 (1988) 131.
% J. Zupan, M. Novic, X. Li, J. Gasteiger, Anal. Chim. Acta, 292 (1994) 219.
%
% 02-10-2006
%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%
clear all;
close all;
echo on;
clc;

% READ THE DATA SET:
sDall = som_read_data('fold1.txt');

% DATA PREPROCESSING
sDall = som_normalize(sDall,'var');
echo off;
disp('  Press any key')
pause;
echo on;
clc

% DEVIDE THE DATA INTO:
% Training set (1/3 of the samples)
sD = som_modify_dataset(sDall, 'extractsamp',[38:115]);

% Test set (2/3 of the samples)
sDtest = som_modify_dataset(sDall, 'removesamp',[38:115]);

echo off;
disp('  Press any key')
pause;
echo on;
clc

% DIVIDE THE VARIABLES INTO INDEPENDENT AND DEPENDENT
% Select which of the variables should be dependent variables (0) and 
% which should be independent variables (1) 

mask = [1; % palmitic
        1; % palmitoleic
        1; % stearic
        1; % oleic
        1; % linoleic
        1; % arachidic (eicosanoic)
        1; % linolenic
        1; % eicosenoic
        0; % North Apulia (NA)
        0; % Calabria (Ca)
        0; % South Apulia (SA)
        0; % Sicily (Si)
        0; % Inner Sardinia (IS)
        0; % Costal Sardinia (CS)
        0; % East Laguria (EL)
        0; % West Laguria (WL)
        0];% Umbria (Um)

echo off;
disp('  Press any key')
pause;
echo on;
clc

% Network size
width  = 22;
length = 21;

% Training lenght
rough   = 24;
fine    = 177;

% SELECT OTHER PARAMETERS OF THE CPNN
init    = 1; % weights initialisation function (1 - linear; 2 - random)
lattice = 1; % neighbourhood (1 - hexagonal; 2 - rectangular)
shape   = 1; % shape of the map (1 - sheet; 2 - cylinder; 3 - toroid)
neighf  = 1; % neighbourhood function (1 - gaussian; 2 - cut gaussian; 3 - ep; 4 - bubble)
learnf  = 1; % learning function (1 - linear; 2 - exponential; 3 - inverse)
labels  = 1; % number of the column with labels 
             % column 1: labels according to regions
             % column 2: number of the samples


echo off;
disp('  Press any key')
pause;
clc

% Training of the CPNN
disp('Training of the CPNN')
disp('Please wait few seconds')

[rmsec, rmsep, sM, tren_pred, test_pred] = ...
         som_counter_prop(sD, sDtest, width, length, rough, fine, mask, init, lattice, shape, neighf, learnf, labels);

disp(' ')
disp('Training of the CPNN is finished. Press any key')

pause;
clc
echo on;

% DISPLAY THE RESULTS
% Show unified distance matrix (UDM) calculated only by INDEPENDENT VARIABLES
f1=figure(1);
scrsz = get(0,'ScreenSize');
set(f1, 'Position',[scrsz(3)/4 scrsz(4)/6 scrsz(3)/2 2*scrsz(4)/3]);

colormap(1-gray)
som_show(sM,'umat','all');
% Show the labels on the map
som_show_add('label',sM.labels, 'textsize',8, 'textcolor','r', 'subplot',1);
title('Unified Distance Matrix (UDM) calculated using independent variables - labeled using TEST SET')
echo off;
disp('  Press any key')
drawnow
pause;
echo on;
clc

% New mask for calculation of the UDM (using only dependent variables)

mask1= [0; % palmitic
        0; % palmitoleic
        0; % stearic
        0; % oleic
        0; % linoleic
        0; % arachidic (eicosanoic)
        0; % linolenic
        0; % eicosenoic
        1; % North Apulia (NA)
        1; % Calabria (Ca)
        1; % South Apulia (SA)
        1; % Sicily (Si)
        1; % Inner Sardinia (IS)
        1; % Costal Sardinia (CS)
        1; % East Laguria (EL)
        1; % West Laguria (WL)
        1];% Umbria (Um)

% Put new mask in the sM variabla
sM.mask = mask1;

% Show unified distance matrix calculated only by DEPENDENT VARIABLES
f2=figure(2);
set(f2, 'Position',[scrsz(3)/4 scrsz(4)/6 scrsz(3)/2 2*scrsz(4)/3]);
colormap(1-gray)
som_show(sM,'umat','all');

echo off;
% Show the labels on the map
som_show_add('label',sM.labels, 'textsize',8, 'textcolor','r', 'subplot',1);
title('UDM calculated using only dependent variables - labeled using TEST SET')

drawnow;
disp('  Press any key')
pause;
echo on;
clc;

% New mask for calculation of the UDM (using all variables)

mask2= [1; % palmitic
        1; % palmitoleic
        1; % stearic
        1; % oleic
        1; % linoleic
        1; % arachidic (eicosanoic)
        1; % linolenic
        1; % eicosenoic
        1; % North Apulia (NA)
        1; % Calabria (Ca)
        1; % South Apulia (SA)
        1; % Sicily (Si)
        1; % Inner Sardinia (IS)
        1; % Costal Sardinia (CS)
        1; % East Laguria (EL)
        1; % West Laguria (WL)
        1];% Umbria (Um)

% Put new mask in the sM variabla
sM.mask = mask2;

% Show unified distance matrix calculated using ALL VARIABLES
f3=figure(3);
set(f3, 'Position',[scrsz(3)/4 scrsz(4)/6 scrsz(3)/2 2*scrsz(4)/3]);
colormap(1-gray)
som_show(sM,'umat','all');

% Show the labels on the map
som_show_add('label',sM.labels, 'textsize',8, 'textcolor','r', 'subplot',1);
title('UDM calculated using ALL variables - labeled using TEST SET')

echo off;
drawnow;
disp('  Press any key')
pause;
echo on;
clc;

% New mask for calculation of the UDM using only one dependent variable
% In this case the dependent variable which corresponds to Inner Sardinia
% is selected.

mask3= [0; % palmitic
        0; % palmitoleic
        0; % stearic
        0; % oleic
        0; % linoleic
        0; % arachidic (eicosanoic)
        0; % linolenic
        0; % eicosenoic
        0; % North Apulia (NA)
        0; % Calabria (Ca)
        0; % South Apulia (SA)
        0; % Sicily (Si)
        1; % Inner Sardinia (IS)
        0; % Costal Sardinia (CS)
        0; % East Laguria (EL)
        0; % West Laguria (WL)
        0];% Umbria (Um)

% Put new mask in the sM variable
sM.mask = mask3;

% Show unified distance matrix calculated only selected DEPENDENT VARIABLE
f4=figure(4);
set(f4, 'Position',[scrsz(3)/4 scrsz(4)/6 scrsz(3)/2 2*scrsz(4)/3]);
colormap(1-gray);
som_show(sM,'umat','all');

% Show the labels on the map
som_show_add('label',sM.labels, 'textsize',8, 'textcolor','r', 'subplot',1);
title('UDM calculated using ONLY ONE dependent variables (the one which corresponds to Inner Sardinia)')
echo off;
drawnow;
disp('  Press any key')
pause;
echo on;
clc;

% Show the weight levels for the intependent variables
f5=figure(5);
set(f5, 'Position',[scrsz(3)/4 scrsz(4)/6 scrsz(3)/2 2*scrsz(4)/3]);
disp('Weigth levels which correspond to input variables')
som_show(sM,'comp',[1:3]);

echo off;
drawnow;
disp('  Press any key')
pause;
echo on;
clc;

% Show the weight levels for the dependent variables
disp('Weigth levels which correspond to dependent variables')
f6=figure(6);
set(f6, 'Position',[scrsz(3)/4 scrsz(4)/6 scrsz(3)/2 2*scrsz(4)/3]);
som_show(sM,'comp',[4:6]);

echo off;
drawnow;
disp('  Press any key')
pause;
clc;
echo on;

% Projection of the data set and the map in the space defined by first 3
% principal components
%
% First we should exclude the dependent variables from the data set (sD) 
% and the weights leveles that correspond dependent variables from the
% trained CPNN (sM)

[Pd,V,me,l] = pcaproj(sD.data(:,1:8),3);  % PC-projection of the data set,
                                          % using only independent
                                          % varibales

% Ecxlude the weights leveles that correspond dependent variables
sMM.type = sM.type;
sMM.labels = sM.labels;
sMM.codebook = sM.codebook(:,1:8);
sMM.neigh = sM.neigh;
sMM.mask = sM.mask(1:8,:);
sMM.trainhist = sM.trainhist;
sMM.name = 'map with removed levels for dependent variables';
sMM.comp_names = sM.comp_names(1:8,:);
sMM.comp_norm = sM.comp_norm(1:8,:);

Pm = pcaproj(sMM,V,me);  % PC-projection of the map into space defined by the data set
Code = som_colorcode(Pm(:,1:2)); % Color coding

% The map and the training data set in the space defined by first 3 PCs
f7=figure(7);
set(f7, 'Position',[scrsz(3)/4 scrsz(4)/6 scrsz(3)/2 2*scrsz(4)/3]);
som_grid(sM,'Coord',Pm,'MarkerColor',Code,'Linecolor','k', 'MarkerSize', 2,'Line',':');

hold on
plot3(Pd(1:8,1),Pd(1:8,2),Pd(1:8,3),'ko')               % NA
plot3(Pd(9:27,1),Pd(9:27,2),Pd(9:27,3),'ro')            % Ca
plot3(Pd(28:38,1),Pd(28:38,2),Pd(28:38,3),'bo')         % Si
plot3(Pd(39:60,1),Pd(39:60,2),Pd(39:60,3),'go')         % IS
plot3(Pd(61:70,1),Pd(61:70,2),Pd(61:70,3),'C*')         % CS
plot3(Pd(71:87,1),Pd(71:87,2),Pd(71:87,3),'r*')         % EL
plot3(Pd(88:103,1),Pd(88:103,2),Pd(88:103,3),'b+')      % WL
plot3(Pd(104:120,1),Pd(104:120,2),Pd(104:120,3),'mo')   % UM
plot3(Pd(121:188,1),Pd(121:188,2),Pd(121:188,3),'g*')   % SA
legend('o (black)  - North Apulia',...
       'o (red)    - Calabria',...
       'o (blue)   - Sicily',...
       'o (green)  - Inner Sardinia',...
       '* (cyan)   - Costal Sardinia',...
       '* (red)    - East Laguria',...
       '+ (blue)   - West Laguria',...
       'o (magenta)- Umbria',...
       '* (green)  - South Apulia',...
       'location', 'NorthEast');
hold off;
axis tight, axis equal
title('Map and the data in PC1-PC2-PC3 defined space')

echo off;
disp('  Press any key')
drawnow
pause;
echo on;
clc

% Returun the original mask into sM for LABELLING
sM.mask = mask;

% Remove previous labels (from the training set) from the map
sM = som_label(sM,'clear','all');

% Labelling of the map using TEST SET samples
sM = som_autolabel(sM, sDtest, 'freq', [labels]);

% NOW put mask1 into sM for DRAWING of the UDM
sM.mask = mask1;

echo off;
disp('  Press any key')
drawnow
pause;
echo on;
clc

% Show unified distance matrix calculated only by DEPENDENT VARIABLES
f8=figure(8);
set(f8, 'Position',[scrsz(3)/4 scrsz(4)/6 scrsz(3)/2 2*scrsz(4)/3]);

colormap(1-gray)
som_show(sM,'umat','all');

% Lablleing of the neurons with the TEST SET
som_show_add('label',sM.labels, 'textsize',8, 'textcolor','r', 'subplot',1);
title('UDM calculated using only dependent variables - labeled using TEST SET')


% Returun the original mask into sM for LABELLING (this time using training set)
sM.mask = mask;

% Remove previous labels from the training set from the map
sM = som_label(sM, 'clear', 'all');

% Labelling of the map using TRAINING SET samples
sM = som_autolabel(sM, sD, 'freq', [labels]);

% NOW put mask1 into sM for DRAWING of the UDM
sM.mask = mask1;

echo off;
disp('  Press any key')
drawnow
pause;
echo on;
clc

% Show unified distance matrix calculated only by DEPENDENT VARIABLES
f9=figure(9);
set(f9, 'Position',[scrsz(3)/4 scrsz(4)/6 scrsz(3)/2 2*scrsz(4)/3]);
colormap(1-gray)
som_show(sM,'umat','all');

% Lablleing of the neurons with the TEST SET
som_show_add('label',sM.labels, 'textsize',8, 'textcolor','r', 'subplot',1);
title('UDM calculated using only dependent variables - labeled using TRAINING SET')
echo off
