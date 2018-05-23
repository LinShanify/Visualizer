
# A Simple Tool for Logging and Plotting Values in Visdom
### Wrapped Functions for Easy ploting and logging when training in pytorch

### 1. Start the visdom server
```
pip3 install -r requirements.txt
python -m visdom.server
```
### 2. import Visualizer and initialize the environment


```python
from Visualizer import Visualizer

#simple init env will set to default value:'main'
vis = Visualizer()

#set up new environment call 'demo'
vis = Visualizer(env='demo')
```

## Logging and Plotting the Ongoing Values in Pytorch (Loss/Accuracy)

### **Single Value Plot `plot`:**  3 parameters:
1. **name**: name for the graph *(string)*
2. **value**: logging value
3. *(optional)* **step**: logging step *(default is 1)*

put the `vis.plot` inside the loop


```python
# simple plot with default logging step
for i in range(0,10,1):
    vis.plot('Single Value Plot Step 1',i/10)
```
![](https://github.com/LinShanify/Visualizer/blob/master/imgs/SingleValuePlot1.png?raw=true)

```python
# simple plot with logging step 10
for i in range(0,10,1):
    vis.plot('Single Value Plot Step 10',i/10, 10)
```
![](https://github.com/LinShanify/Visualizer/blob/master/imgs/SingleValuePlot10.png?raw=true)

### **Multi-Value Plots into Separate Graphs `plot_all`:**  2 parameters:
1. **dict**: dictionary contains each name and value
```
vis.plot_all({'Acc':0.9,'Loss':0.1})
```
3. *(optional)* **step**: logging step *(default is 1)*


```python
# Plot Acc graph and Loss graph at the same time with default logging step
for i in range(0,10,1):
    loss = {'Acc':i/10,
            'Loss':1-i/10}
    vis.plot_all(loss)
```
![](https://github.com/LinShanify/Visualizer/blob/master/imgs/Acc.png?raw=true) ![](https://github.com/LinShanify/Visualizer/blob/master/imgs/Loss.png?raw=true)

### **Multi-Value Plots in One Single Graph `plot_combine`:**  3 parameters:
1. **name**: name for the graph *(string)*
2. **dict**: dictionary contains each name and value
```
vis.plot_combine('Combine Plot',{'Acc':0.9,'Loss':0.1})
```
3. *(optional)* **step**: logging step *(default is 1)*


```python
# Plot acc value and loss value in one graph with default logging step
for i in range(0,10,1):
    loss = {'Acc':i/10,
            'Loss':1-i/10}
    vis.plot_combine('Combine Plot',loss)
```
![](https://github.com/LinShanify/Visualizer/blob/master/imgs/CombinePlot.png?raw=true)
___
## Logging String in Pytorch
### **Text Log `log`:** 3 parameters:
1. **info**: logging info
2. **name**: name for the visdom window


```python
for i in range(0,10,1):
    loss = {'Acc':i/10,
            'Loss':1-i/10}
    vis.log(loss)
```
![](https://github.com/LinShanify/Visualizer/blob/master/imgs/Log.png?raw=true)
___
## Display Pytorch Tensor as Images
### **Image Display `img`:**  2 parameters:


1. **name**: name for the image *(strinf)*
2. **img_**: pytorch tensors: single image or multi-images
``` pytorch
t.Tensor(64,64)
t.Tensor(3,64,64)
t.Tensor(100,1,64,64)
t.Tensor(100,3,64,64)
```
3. *(optional)* **nrow**: number of image per row *(integer)*


```python
#Plot Single Images
import torch
from PIL import Image
import numpy as np
img = Image.open('imgs/Lenna.png')
arr = np.array(img).transpose((2, 0, 1))
tensor = torch.from_numpy(arr)
vis.img('SingleImage',tensor)
```
![](https://github.com/LinShanify/Visualizer/blob/master/imgs/Lenna.png?raw=true)

```python
#Plot Multi Images (4D tensor)
img = Image.open('imgs/Lenna.png')
arr = np.array(img).transpose((2, 0, 1))
tensor = torch.from_numpy(arr)
tensor4D = tensor.unsqueeze(0)
for i in range(5):
    tensor4D=torch.cat((tensor4D,tensor.unsqueeze(0)),0)
vis.img('MultiImage',tensor4D, nrow=3)
```
![](https://github.com/LinShanify/Visualizer/blob/master/imgs/MultiImage.jpg?raw=true)
___
### **Check Visdom Connection**: `check_connection`


```python
vis.check_connection()
```




    True
    


![](https://github.com/LinShanify/Visualizer/blob/master/imgs/demo.png?raw=true)
