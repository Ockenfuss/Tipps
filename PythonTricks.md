# Python Tricks


<!-- @import "[TOC]" {cmd="toc" depthFrom=1 depthTo=6 orderedList=false} -->

<!-- code_chunk_output -->

- [Python Tricks](#python-tricks)
  - [Python General](#python-general)
    - [Installation](#installation)
    - [General](#general)
      - [Functions:](#functions)
      - [Variable reference in Python](#variable-reference-in-python)
      - [Datatypes](#datatypes)
      - [Strings](#strings)
      - [Arrays](#arrays)
      - [Lists](#lists)
      - [File paths](#file-paths)
    - [Datetime module](#datetime-module)
      - [Datum aus String](#datum-aus-string)
      - [String aus Datum:](#string-aus-datum)
      - [Differenz in Sekunden:](#differenz-in-sekunden)
      - [Zeiten addieren:](#zeiten-addieren)
      - [Zeiten Plotten](#zeiten-plotten)
    - [time Module](#time-module)
  - [Numpy](#numpy)
      - [Numpy I/O](#numpy-io)
      - [Numpy Arrays erstellen](#numpy-arrays-erstellen)
      - [Indexing](#indexing)
      - [Basic Slicing](#basic-slicing)
      - [Numpy sortieren](#numpy-sortieren)
      - [Numpy Array reshape](#numpy-array-reshape)
      - [stack/extend/combine numpy arrays](#stackextendcombine-numpy-arrays)
      - [combine multidimensional arrays](#combine-multidimensional-arrays)
      - [Numpy Datentypen](#numpy-datentypen)
      - [Numpy Funktionen](#numpy-funktionen)
      - [Statistics](#statistics)
      - [Fourier Transformations](#fourier-transformations)
  - [Pandas](#pandas)
      - [Create Data Frame](#create-data-frame)
      - [read from file](#read-from-file)
      - [acces element](#acces-element)
      - [boolean indexing](#boolean-indexing)
      - [get columns](#get-columns)
      - [Categories in one column](#categories-in-one-column)
      - [iterration over rows](#iterration-over-rows)
      - [Groupby](#groupby)
  - [Pyplot/Matplotlib](#pyplotmatplotlib)
      - [create plot](#create-plot)
      - [Axen und Ticks](#axen-und-ticks)
      - [Beschriftung mit Latex](#beschriftung-mit-latex)
      - [Legende](#legende)
      - [linestyle cycle](#linestyle-cycle)
      - [colorbar](#colorbar)
        - [Create colorbar](#create-colorbar)
        - [Colorbar limits and scale](#colorbar-limits-and-scale)
        - [Color cycle setzen](#color-cycle-setzen)
      - [Zweite Axe rechts:](#zweite-axe-rechts)
      - [Figuren](#figuren)
        - [Kreis plotten](#kreis-plotten)
      - [Save File Location default and default extension (pdf)](#save-file-location-default-and-default-extension-pdf)
      - [Backend](#backend)
      - [Arange subplots](#arange-subplots)
      - [Text and annotations](#text-and-annotations)
      - [Plot types](#plot-types)
        - [Images (2D Verteilung) plotten](#images-2d-verteilung-plotten)
      - [Animationen](#animationen)
  - [NETCDF](#netcdf)
  - [Image processing](#image-processing)
      - [Convolution](#convolution)
  - [Create your own modules](#create-your-own-modules)
      - [import from parent directory](#import-from-parent-directory)
      - [How to write proper docstrings for functions/classes:](#how-to-write-proper-docstrings-for-functionsclasses)

<!-- /code_chunk_output -->


## Python General

### Installation

`sudo apt install python3`
Pip: `sudo apt install python3-Pip`
install package with pip: use `--user` flag if you have problems
also possible: install with `python3 -m pip install`
list packages (python3 -m) `pip list`
upgrade pip: 

### General

beendet ein Python Program 
<!-- Stop Interrupt -->
```python
import sys
sys.exit()
```

#### Functions: 
often use "args" and "kwargs" (=keyword arguments) to pass additional arguments to new function.
Syntax: *args or **kwargs This is part of a more general behaviour: * unpacks an array or list so its elements can be function arguments, ** does the same with a dictionary (creating named arguments)

#### Variable reference in Python
in general, every object has an id (similar to pointer in C)
Function arguments are in general by reference, i.e. inside the function, we are dealing with a new reference for the original object
Generally, expressions are evaluated from the right
Expressions like a=a+10 (a can be numpy array!): Create a new, deep copy and assign its reference to a

#### Datatypes
convert to integer
`int(x)`

#### Strings
combine strings
```python
test="Hallo"+"du"
```
print array elements with the given separator (if necessary: use `[str(i) for i in arr] first`)
```python
",".join(arr)
```
remove occurances at beginning or end
```python
string.strip("\n")
```

#### Arrays
returns the position as well as the value of the array
```python
for index, values in enumerate(array, start)
```
`numpy.ndenumerate:`
Extremely useful for looping over multidimensional arrays! Gives index as tuple.
if index should be single value: `enumerate(array.flatten())`

Converts scalars to array and leaves everything else untouched=>Usefull for functions to handle scalars as well arrays
```python
np.atleast_1d(arrays)
```
convert to 2d by "adding one pair of brackets": array2d=[array1d] or array2d=\[[scalar]]
```python
np.atleast_2d(array)
```

#### Lists
Like Arrays, but can (1) contain different data types and (2) be extended dynamically
`liste=[]`
`liste.append("Hallo")`
`liste.append(liste2)`list of lists
#### File paths
```python
import os.path
os.path.join()
os.path.splitext(filename)#tuple with Path+Name and Extension
```
### Datetime module
Datum und Zeit
Übersicht: https://www.programiz.com/python-programming/datetime
Vergleiche: PlotSeries.py
```python
import datetime
from dateutil.parser import parse
```
#### Datum aus String 
```python
strptime()
time = np.array([datetime.datetime.strptime(TimeString[i],"%m.%d.%y %H:%M:%S") for i in range(len(TimeString))])
"%m.%d.%y %H:%M:%S.%f"#falls mit Milliseconds
```
####String aus Datum:
```python
s=time.strftime("%H:%M")
```

####Differenz in Sekunden: 
(a-b).total_seconds()
```python
t=np.array([(time[i]-time[i+1]).total_seconds() for i in range(len(time))])
```

####Zeiten addieren:
```python
time=time+timedelta(hours=7)
```

#### Zeiten Plotten 
"time" muss dabei wieder ein Array aus datetime-Objekten sein.
Vergleiche: PlotJPLResults.py

```python
import matplotlib.dates as dates
ax.xaxis.set_major_formatter(dates.DateFormatter('%H:%M'))
```


Wichtig: Wenn Daten außer einer Uhrzeit auch ein Datum enthalten, muss dieses in den Limits auch angegeben werden.

```python
ax.set_xlim(datetime.datetime.strptime("8.21.2017 15:00","%m.%d.%y %H:%M:%S"),datetime.datetime.strptime("8.21.2017 16:00","%m.%d.%y %H:%M:%S"))
```

Abstände der Labels (hier 2 Minuten)
```python
ax.xaxis.set_major_locator(dates.MinuteLocator(interval=2))
plt.plot(time,d)
```






### time Module

```python
import time
time.sleep(5.5)#5.5 seconds pause
```











##Numpy
####Numpy I/O
```python
np.save("folder/file",array)#save as binary numpy object
array=np.load("folder/file.npy")#load binary object
array=np.genfromtxt("file")#load from asciifile
```

####Numpy Arrays erstellen
```python
np.zeros(100)
np.ones(100)
```
make e.g. two 2D grids of two 1D arrays. With ij, every column of xgrid equals x, every row of ygrid equals y. This is the "matrix interpretation".
```python
xgrid, ygrid=np.meshgrid(x,y,indexing='ij')
```

####Indexing
Boolsche Indizes: auswählen eines bestimmten Teilarrays
<,>,== geht direkt, für Verknüpfung (Oder, Und) mehrerer Ausdrücke:
```python
b=np.logical_and(t<deltat, t>=0)
print(t[b])
```
Elementwise boolean operations: https://docs.scipy.org/doc/numpy-1.13.0/reference/routines.logic.html
```python
np.logical_and(a,b)#perform logical and elementwise
```
####Basic Slicing
```python
array[start:stop:step]#if start/stop<0, replace by start/stop+n with n the dimension of the axis
```


####Numpy sortieren
argsort: Liefert array mit den Indizes in der sortierten Reihenfolge => einsetzen liefert sortiertes Array
```python
ind=np.argsort(a)
print(a[ind])#sortiert
```


Numpy Elemente finden: np.where gibt array zurück mit allen reihenindizes in einem array und allen columnindizes in zweitem.Bsp:
```python
a=[1,1,1,2,2,3]
indexarray=np.where(a==1)#gibt array: (array([0, 1, 2]),)=> indexarray[0] ist also [0,1,2] für die vorkommen von "1" in a
np.amax(array)#maximum of array
np.argmax(array)#index of maximum
```

####Numpy Array reshape
Imagine the old array to be flattened first: First, the last dimension is counted up (0,1,2,3...), then the second last is raised +1 and the last counted up again and so on.
Secondly, the resulting 1d array is filled into the new one by the same principle: the first (50) elements of the 1D array are put in the last dimension (0-50), then the second last dimension (0-400) is raised +1 and the next (50) elements are put into the last dimension (0-50) again, and so on.
```python
new=old.reshape((3,400,50))
```

####stack/extend/combine numpy arrays
```python
np.column_stack((arr1, arr2, arr3))#combine 1D arrays to 2D
np.row_stack((arr1, arr2, arr3))
np.repeat(arr1[:, :, np.newaxis], n_repeat, axis=2)#repeat a given array into a new direction
```
####combine multidimensional arrays
```python
np.concatenate((arr1, arr2), axis=1)#must have same dimensions except on the concatenation axis
```

####Numpy Datentypen
```python
arr2=arr1.astype(int)#Array conversion
```

####Numpy Funktionen
```python
np.mean(Array, Axis=0)#Numpy
np.histogram2d(x,y,weights=z, bins=10)#1 or 2d histogram in numpy
np.arctan2(y,x)#Gives the angle of a point x,y including information about the quadrant (be aware: arctan2(y,x), NOT x,y!)
#x=1, y=1: 45°, x=-1, y=1: 135°, x=-1, y=-1: -135°, x=1, y=-1: -45° (upper half (y>0) positive angles, lower half negative angles)
#Easy transformation 0 to 360: (360+angle)%360
np.interp(x_new, x_old, y_old)#perform linear interpolation of the old values at the adjacent old points. x_old must be increasing if not specified further!
```

####Statistics
```python
np.random.rand(*arr.shape)#produce random numbers between 0 and 1 in an array with the given dimensions
np.random.normal(0,1,(2,2))#produce normal distributed values in an array with the given shape
np.random.multivariate_normal(a,b)#produce a set of numbers (vector), such that these variables in the limit of many observations have the given covariance matrix b (around the mean vector a).
```

####Fourier Transformations
```python
sig_f=np.fft.fft(sig)#Simple discrete Fourier transform
sig2d_f=np.fft.fft2(sig2d)#For 2d arrays (images)
```
Sig_f contains the amplitudes corresponding to the frequencies given from `np.fft.fftfreq(N, d=1.0)`, with `N=len(sig)`. The general format is [k0, k1, ..., kN/2, k(-N/2), ... k(-1)], and the values are just kn=n/(N*d). Therefore, to get the real frequencies k we usually want in e^(ikx), we have to set d=dx/(2 * pi), with dx the spacing of the real space points in `sig`.


##Pandas
My personal style guide: Stick to numpy arrays. If necessary (for comfort), make up columnnames like A=0, B=1 so you can call array[4,A].

####Create Data Frame
```python
test=[[4,5,6],[1,2,3]]
testdf=pd.DataFrame(test,columns=['A','B','C'])
```
####read from file
```python
data=pd.read_csv("FileName")
```

####acces element
both work with boolean indexing
```python
data.loc['A','B']#if you use column/row names
data.iloc[1:2,3:4]#if you use indices
```
important: iloc chooses based on the POSITION, loc is based on the LABEL! Can be confusing because, e.g. for rows, integers can be labels as well.

####boolean indexing
```python
 s[(s < -1) | (s > 0.5)]
```

####get columns
```python
datadp.columns
```

####Categories in one column
```python
np.unique(testdf.A)
```

####iterration over rows
```python
for index, line in df.iterrows():
    print(line)
```
####Groupby
combine rows which have a common feature into subgroups
(https://pandas.pydata.org/pandas-docs/stable/user_guide/groupby.html)
```python
datgroup=data.groupby("Label")
```
if groupby by two labels: multindices occur: https://www.datacamp.com/community/tutorials/pandas-multi-index
* acces
```python
datgroup.get_group["A"]
```
* iterate over groups
```python
for name, group in enumerate(datgroup):
    print(group)
```


##Pyplot/Matplotlib
https://matplotlib.org/faq/usage_faq.html#usage
Basic structure: A Figure object is the empty window which contains the plots.
In the figure is a certain number of Axes objects, the actual "plots".
Each Axes object can have for example two or three Axis-objects, the actual "axes of the plots". Axes!=Axis!!!

####create plot
```python
fig,ax=plt.subplots()
fig2, ax2=plt.subplots(nrows=2, ncols=2)#ax2: either 1D or 2D array => use atleast2d().T if necessary
fig.suptitle('This is a somewhat long figure title', fontsize=16)
```


####Axen und Ticks
```python
ax.set_ylim(1e-7,5e1)#Limits
ax.set_xticks([1,2,3])#Ticks setzen. ax.get_xticks() liefert ticks
ax.tick_params(labelsize=16)
ax.yaxis.tick_right()#Ticks rechts setzen
ax.yaxis.set_label_coords(-0.1, 0.5)#exakte position
ax.get_yaxis().set_visible(False)#hide ticks/axis

ax2.set(ylabel="ratio", title="Titel")#Beschriftung
ax.grid(True, which='major')#Gitter
```
####Beschriftung mit Latex
```python
plt.rc('text', usetex=True) #always necessary?
ax1.set_xlabel(r'Irradiance [$\frac{mW}{m^{2}nm}]$', fontsize=18)
ax.plot(x,y, label=r'$\phi={0}\pi$'.format(i))#use variable in latex label

```
####Legende
```python
legend2 = ax2.legend(loc='lower right', shadow=True, fontsize='medium')#Legende
dummy_lines = []#Legende nur mit Linestyle
dummy_lines.append(ax2.plot([],[], c="black", linestyle ="-", linewidth=1.2)[0])
dummy_lines.append(ax2.plot([],[], c="black", linestyle ="--", linewidth=1.6)[0])
legend2 = ax2.legend([dummy_lines[i] for i in [0,1]], ["Measurements", "Simulation"], loc="upper left")
ax2.add_artist(legend2)
```

####linestyle cycle
```python
from itertools import cycle
lines = ["-","--","-.",":"]
linecycler = cycle(lines)
ax.plot(x,y, linestyle=next(linecycler))
```


####colorbar

#####Create colorbar
They always live in their own axes object!
```python
cbar=fig.colorbar(im)#If there are multiple axes and we want to assign a colorbar to one, we can create a small axes next to the axes with the image:
cbar.set_label("Label")
cbar.ax.tick_params(labelsize=10)
from mpl_toolkits.axes_grid1 import make_axes_locatable
divider = make_axes_locatable(ax[0])#If there are multiple axes 0,1,2,3...
cax = divider.append_axes('right', size='5%', pad=0.05)
fig.colorbar(contour, cax=cax)
```
#####Colorbar limits and scale
https://matplotlib.org/users/colormapnorms.html
```python
import matplotlib.colors as colors
ax.matshow(Mat, norm=colors.SymLogNorm(linthresh=0.003, linscale=1.0, vmin=-2.0, vmax=2.0))#log scale with symmetric area around 0. linthresh: the value, where the linear range starts. linscale: if 1.0, the linear range has the width of one order of magnitude on the colorbar.
```

#####Color cycle setzen
```python
cm = plt.get_cmap('gist_rainbow')#Cmaps: https://matplotlib.org/examples/color/colormaps_reference.html
ax.set_prop_cycle(plt.cycler('color', [cm(1.*i/15) for i in range(15)]))
```


####Zweite Axe rechts:
```python
ax2=ax.twinx()#twiny() for axis on top
```

####Figuren
```python
ax.axvline(x=20)#axhline(y=20)
```
#####Kreis plotten
```python
circle1 = plt.Circle((1000, 1000), 30, color='r', fill=True)
ax.add_artist(circle1)
```

####Save File Location default and default extension (pdf)
```python
plt.rcParams["savefig.directory"] =os.path.dirname(os.path.abspath(__file__))#need to "import os"
plt.rcParams["savefig.format"]="pdf"
fig.savefig("Plot1.pdf")#save plot
```
####Backend
```python
matplotlib.use("Agg")#specify backend, before importing pyplot! Important if Display variable is not set. Alternative: TkAgg
matplotlib.get_backend()#get backend
print(matplotlib.matplotlib_fname()) #find matplotlibrc
```


####Arange subplots
fig, ax=plt.subplots(2,2, figsize=(4,3))#Easiest way
fig.subplots_adjust(hspace=0.1,wspace=0.1)#Adjust height and width spacing in units of mean axis length
fig = plt.figure(figsize=(4,3))#Define grid and specify over how many cells axes spread
grid = plt.GridSpec(4, 3, hspace=0.4, wspace=0.2)
gs = gridspec.GridSpec(4, 2, width_ratios=[1, 1], height_ratios=[1, 10,10, 10])#another possibility
ax1 = fig.add_subplot(grid[:-1,:])
ax2 = fig.add_subplot(grid[-1, :])

####Text and annotations
ax1.text(1,2,"Hallo", rotation=45)#Annotation, Text
ax1.annotate("Hallo", xy=(0.5,0.5), xytext=(0.6,0.6), xycoords='figure fraction')#more functions than simple text, e.g. make arrows and give coordinates in different formats

####Plot types
n, bins, _ =ax2.hist(distance,bins=100, weights=values, range=(-60,60), density=True)#histogramm. If density=True, the y axis values are in units of %/xaxis, i.e. np.sum(n[-1]*np.diff(bins))=1.0. (n[-1] in the case of multiple categories and stacked=True, for a plot with one category it is just n*np.diff)
im=ax2.hist2d(distance,bins=100, weights=values, range=(-60,60))[3]#histogramm 2d, object for colorbar is the fourth returned object

#####Images (2D Verteilung) plotten
```python
from matplotlib.colors import LogNorm#falls mit LogNorm
im=ax.imshow(b, cmap='gray', interpolation='none', norm=LogNorm())#b: 2D Array mit Pixelwerten
im.cmap.set_under('k',1.) 

contour=ax.contour(x,y,z, colors='k')#Contour plot: Draw height lines ('isobares')
ax.clabel(contour, colors = 'k', fmt = '%2.1f', fontsize=12)#write height values to lines

ax.contourf(x,y,z, cmap='Greys')#Draw a filled contour plot, i.e. with areas rather than lines.

```


####Animationen
Animation Artist:
```python
import matplotlib.animation as animation
fig, ax=plt.subplots()
ims=[]
for i in range(1,200):
    title = plt.text(0.5,1.01,i, ha="center",va="bottom", transform=ax.transAxes)
    im=ax.imshow(image[i], cmap="Greys_r")#pcolormesh o.ä. works as well
    ims.append([im,title])
ani = animation.ArtistAnimation(fig, ims, interval=30, blit=True, repeat_delay=40)
ani.save('SimpleBox.mp4')
plt.show()
```
Animation Function:
```python
from matplotlib.animation import FuncAnimation
def create_1d_animation(fig, ax, x, values, interval):
    #format [frame, position]
    line, = ax.plot([], [], lw=1)
    def animate(i):
        a = x[i]
        b = values[i]
        line.set_data(a, b)
        return line,
    print(x.shape[0])
    anim = FuncAnimation(fig, animate, frames=x.shape[0], interval=interval, blit=True)
    return anim
```

##NETCDF
Idea: A NETCDF File consits of variables. Each variable can implement a certain number of dimensions (like time, lat, lon).
Dimensions are essentially also variables itself ("coordinate variables")

##Image processing
####Convolution
```python
from scipy import signal as sig
AcM=sig.convolve2d(A,M, mode='full', boundary='fill', fillvalue=0)
```
boundary: How boundary conditions are treated: fill missing values on the rim with a value, use periodic boundary conditions ('wrap') or mirror the values directly at the rim to the outside ('symm')
mode: Size of the resulting array, 'full', "valid", or "same"


##Create your own modules
https://www.digitalocean.com/community/tutorials/how-to-write-modules-in-python-3
All about paths and importing from other directories:
https://chrisyeh96.github.io/2017/08/08/definitive-guide-python-imports.html

Configure paths:
* option1: define PYTHONPATH variable in .bashrc (problem: might be system dependent, dotfiles are synchronized via github)
for intellisense: set in settings json: "python.autoComplete.extraPaths": ["/home/p/Paul.Ockenfuss/Documents/CodeTemplates/Python"]
* option2: set a link in one of the default locations (import sys, print(sys.path))
e.g. in /usr/lib/python3/dist-packages
intellisense should work automatically

####import from parent directory
```python
def load_src(name, fpath):
    import os, imp
    p = fpath if os.path.isabs(fpath) \
        else os.path.join(os.path.dirname(__file__), fpath)
    return imp.load_source(name, p)
load_src("Tools", "/example/example/Tools.py")#absolute or relative path possible!
import Tools
```

####How to write proper docstrings for functions/classes:
```python
https://www.python.org/dev/peps/pep-0257/
def complex(real=0.0, imag=0.0):
    """Form a complex number.

    Keyword arguments:
    real -- the real part (default 0.0)
    imag -- the imaginary part (default 0.0)
    """
    if imag == 0.0 and real == 0.0:
        return complex_zero
```