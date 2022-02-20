# matlab.md

Most professional data scientists, physicists and astronomers use the open-source Python as their main interpreted language, with substantial minorities using open-source languages [R](https://www.r-project.org/) [Julia](https://julialang.org/) and the closed-source [IDL](https://www.l3harrisgeospatial.com/Software-Technology/IDL). 

It has been common for universities to teach students Matlab, but this is increasingly uncommon in physics outside of engineering contexts, and requires expensive closed-source licenses. I personally don't recommend you practise Matlab unless you are going mainly into engineering, but if you are already familiar with it, it is well suited to this project. Here are some resources for using Matlab for the Distance Ladder project.

## Loading data

Script to read the data given in the `allpoints` and `fuzzypoints` data files and convert them into `.csv` files according to the camera location.

Files need to be saved as `allpoint.txt` and `allfuzzy.txt` in the same folder directory as this script is saved in.
Thanks for Kyle Clunies-Ross for writing this script!


```
clc;
clear all;

%% Importing data
filename1 = 'allpoint.txt';
filename2 = 'allfuzzy.txt';

startRow = 2;
endRow = inf;

%creating file IDs
fileID1 = fopen(filename1,'r');
fileID2 = fopen(filename2,'r');
%set up the formatting for the arrays
formatSpec = '%5C%11f%8f%9f%9f%9f%8f%f%[^\n\r]';

%Making an array from the files
dataArray1 = textscan(fileID1, formatSpec, endRow(1)-startRow(1)+1, 'Delimiter', '', 'WhiteSpace', '', 'TextType', 'string', 'EmptyValue', NaN, 'HeaderLines', startRow(1)-1, 'ReturnOnError', false, 'EndOfLine', '\r\n');
dataArray2 = textscan(fileID2, formatSpec, endRow(1)-startRow(1)+1, 'Delimiter', '', 'WhiteSpace', '', 'TextType', 'string', 'EmptyValue', NaN, 'HeaderLines', startRow(1)-1, 'ReturnOnError', false, 'EndOfLine', '\r\n');
for block=2:length(startRow)
    frewind(fileID1);
    frewind(fileID2);
    dataArrayBlock1 = textscan(fileID1, formatSpec, endRow(block)-startRow(block)+1, 'Delimiter', '', 'WhiteSpace', '', 'TextType', 'string', 'EmptyValue', NaN, 'HeaderLines', startRow(block)-1, 'ReturnOnError', false, 'EndOfLine', '\r\n');
    dataArrayBlock2 = textscan(fileID2, formatSpec, endRow(block)-startRow(block)+1, 'Delimiter', '', 'WhiteSpace', '', 'TextType', 'string', 'EmptyValue', NaN, 'HeaderLines', startRow(block)-1, 'ReturnOnError', false, 'EndOfLine', '\r\n');
    for col=1:length(dataArray1)
        dataArray1{col} = [dataArray1{col};dataArrayBlock1{col}];
    end
    for col=1:length(dataArray2)
        dataArray2{col} = [dataArray2{col};dataArrayBlock2{col}];
    end
end

%Close files
fclose(fileID1);
fclose(fileID2);

%set up tables from the arrays
allpointData = table(dataArray1{1:end-1}, 'VariableNames', {'Cam','X','Y','BlueFlx','GreenFlx','RedFlx','Par','RV'});
allfuzzyData = table(dataArray2{1:end-1}, 'VariableNames', {'Cam','X','Y','BlueFlx','GreenFlx','RedFlx','Size','RV'});

%% Putting the data from the tables into individual matrices
Name1 = ["Cam","X","Y","BlueFlx","GreenFlx","RedFlx","Par","RV"]; %writing headers
for i = 1:size(allpointData,2)
   Back1(1,i) = Name1(1,i);
   Front1(1,i) = Name1(1,i);
   Left1(1,i) = Name1(1,i);
   Right1(1,i) = Name1(1,i);
   Up1(1,i) = Name1(1,i);
   Down1(1,i) = Name1(1,i);
end

Name2 = ["Cam","X","Y","BlueFlx","GreenFlx","RedFlx","Size","RV"]; %writing headers
for i = 1:size(allpointData,2)
   Back2(1,i) = Name2(1,i);
   Front2(1,i) = Name2(1,i);
   Left2(1,i) = Name2(1,i);
   Right2(1,i) = Name2(1,i);
   Up2(1,i) = Name2(1,i);
   Down2(1,i) = Name2(1,i);
end

a = 1; %These variables used to index the matrices
b = 1;
c = 1;
d = 1;
e = 1;
f = 1;

%Writing these values to matrices
for i = 1:size(allpointData,1)
    if allpointData{i,1} == 'Back'
        a = a + 1;
        for n = 1:size(allpointData,2)
            Back1(a,n) = string(allpointData{i,n});
        end
    elseif allpointData{i,1} == 'Front'
        b = b + 1;
        for n = 1:size(allpointData,2)
            Front1(b,n) = string(allpointData{i,n});
        end  
    elseif allpointData{i,1} == 'Left'
        c = c + 1;
        for n = 1:size(allpointData,2)
            Left1(c,n) = string(allpointData{i,n});
        end
    elseif allpointData{i,1} == 'Right'
        d = d + 1;
        for n = 1:size(allpointData,2)
            Right1(d,n) = string(allpointData{i,n});
        end
    elseif allpointData{i,1} == 'Up'
        e = e + 1;
        for n = 1:size(allpointData,2)
            Up1(e,n) = string(allpointData{i,n});
        end
    elseif allpointData{i,1} == 'Down'
        f = f + 1;
        for n = 1:size(allpointData,2)
            Down1(f,n) = string(allpointData{i,n});
        end
    end
end

a = 1; %Resetting these variables
b = 1;
c = 1;
d = 1;
e = 1;
f = 1;

for i = 1:size(allfuzzyData,1)
    if allfuzzyData{i,1} == 'Back'
        a = a + 1;
        for n = 1:size(allfuzzyData,2)
            Back2(a,n) = string(allfuzzyData{i,n});
        end
    elseif allfuzzyData{i,1} == 'Front'
        b = b + 1;
        for n = 1:size(allfuzzyData,2)
            Front2(b,n) = string(allfuzzyData{i,n});
        end  
    elseif allfuzzyData{i,1} == 'Left'
        c = c + 1;
        for n = 1:size(allfuzzyData,2)
            Left2(c,n) = string(allfuzzyData{i,n});
        end
    elseif allfuzzyData{i,1} == 'Right'
        d = d + 1;
        for n = 1:size(allfuzzyData,2)
            Right2(d,n) = string(allfuzzyData{i,n});
        end
    elseif allfuzzyData{i,1} == 'Up'
        e = e + 1;
        for n = 1:size(allfuzzyData,2)
            Up2(e,n) = string(allfuzzyData{i,n});
        end
    elseif allfuzzyData{i,1} == 'Down'
        f = f + 1;
        for n = 1:size(allfuzzyData,2)
            Down2(f,n) = string(allfuzzyData{i,n});
        end
    end
end

%% Writing all this data to individual csv files
file1 = fopen('allpointBack.csv','w');
file2 = fopen('allpointDown.csv','w');
file3 = fopen('allpointFront.csv','w');
file4 = fopen('allpointLeft.csv','w');
file5 = fopen('allpointRight.csv','w');
file6 = fopen('allpointUp.csv','w');
file7 = fopen('allfuzzyBack.csv','w');
file8 = fopen('allfuzzyDown.csv','w');
file9 = fopen('allfuzzyFront.csv','w');
file10 = fopen('allfuzzyLeft.csv','w');
file11 = fopen('allfuzzyRight.csv','w');
file12 = fopen('allfuzzyUp.csv','w');
fprintf(file1,'%s,%s,%s,%s,%s,%s,%s,%s,\n',Back1.');
fprintf(file2,'%s,%s,%s,%s,%s,%s,%s,%s,\n',Down1.');
fprintf(file3,'%s,%s,%s,%s,%s,%s,%s,%s,\n',Front1.');
fprintf(file4,'%s,%s,%s,%s,%s,%s,%s,%s,\n',Left1.');
fprintf(file5,'%s,%s,%s,%s,%s,%s,%s,%s,\n',Right1.');
fprintf(file6,'%s,%s,%s,%s,%s,%s,%s,%s,\n',Up1.');
fprintf(file7,'%s,%s,%s,%s,%s,%s,%s,%s,\n',Back2.');
fprintf(file8,'%s,%s,%s,%s,%s,%s,%s,%s,\n',Down2.');
fprintf(file9,'%s,%s,%s,%s,%s,%s,%s,%s,\n',Front2.');
fprintf(file10,'%s,%s,%s,%s,%s,%s,%s,%s,\n',Left2.');
fprintf(file11,'%s,%s,%s,%s,%s,%s,%s,%s,\n',Right2.');
fprintf(file12,'%s,%s,%s,%s,%s,%s,%s,%s,\n',Up2.');
fclose(file1);
fclose(file2);
fclose(file3);
fclose(file4);
fclose(file5);
fclose(file6);
fclose(file7);
fclose(file8);
fclose(file9);
fclose(file10);
fclose(file11);

```