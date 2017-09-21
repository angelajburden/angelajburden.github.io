## Context
+ To measure properties of our universe galaxy surveys set up large telescopes to observe the light of millions of objects. By measuring the distribution of different classes of objects we can infer different things about the universe such as what it is made up of. 

+ We can measure the light using two different methods, photometric and spectroscopic observations. 

    - Photometic observations measure the amount of light passing through coloured filters. Therefore you will get the intensity of light but only as a function of a few wavelengths. However you can observe many objects at once.

    - For spectroscopic observations the light travels through a spectrometer and you recover the continuous intensity of light as a function of wavelength. For each object you get a lot more information but this method is more expensive and not as many objects can be observed at one time.

+ As spectroscopic observations are more expensive we want to be sure that we point our optical fibers on the objects that we wish to obtain spectra for. Therefore we first conduct photometric observations of mny objects in the region of the sky we are interested in, and from these measurement try and distinguish between objects so that we know the positions of objects we wish to target with our spectroscopic survey.


## Aim 
Given a list of objects with similar photometric attributes, i.e. no distinct characteristics that we can use to make cuts, train a neural network to identify quasars from other objects. 

## Data
The data is publicly available here 

http://skyserver.sdss.org/dr7/en/tools/search/sql.asp

+ We choose 30000 known QSO (quasars) and their properties using the SQL commands

```
SELECT TOP 30000 s.z,s.zErr, s.specClass, p.psfMag_u,p.psfMag_g,p.psfMag_r,p.psfMag_i,p.psfMag_z, p.psfMagErr_u,p.psfMagErr_g,p.psfMagErr_r,p.psfMagErr_i,p.psfMagErr_z FROM BESTDR7 as s

JOIN BESTDR7 AS p ON s.bestObjID = p.objID

WHERE (s.specClass = dbo.fSpecClass('QSO') OR s.specClass = dbo.fSpecClass('HIZ_QSO')) 
AND s.zConf>0.35 AND s.z>0.5 AND s.z<5 AND p.psfMag_u>0 AND p.psfMag_u<26 AND p.psfMag_g>19 
AND p.psfMag_g<25 AND p.psfMag_r>0 AND p.psfMag_r<24 AND p.psfMag_i>0 AND p.psfMag_i<24 
AND p.psfMag_z>0 AND p.psfMag_z<24 AND p.psfMagErr_u>0 AND p.psfMagErr_g>0 
AND p.psfMagErr_r>0 AND p.psfMagErr_i>0 AND p.psfMagErr_z>0
```

+ We choose 30000 other point-like objects (PLO) using the following commands

```
SELECT TOP 30000 s.z,s.zErr, s.specClass, p.psfMag_u,p.psfMag_g,p.psfMag_r,p.psfMag_i,p.psfMag_z, p.psfMagErr_u,p.psfMagErr_g,p.psfMagErr_r,p.psfMagErr_i,p.psfMagErr_z FROM BESTDR7 as s

JOIN BESTDR7 AS p ON s.bestObjID = p.objID

WHERE (s.specClass = dbo.fSpecClass('STAR')) 
AND p.psfMag_u>0 AND p.psfMag_u<26 AND p.psfMag_g>19 AND p.psfMag_g<25 AND p.psfMag_r>0 
AND p.psfMag_r<24 AND p.psfMag_i>0 AND p.psfMag_i<24 AND p.psfMag_z>0 AND p.psfMag_z<24 
AND p.psfMagErr_u>0 AND p.psfMagErr_g>0 AND p.psfMagErr_r>0 AND p.psfMagErr_i>0 AND p.psfMagErr_z>0 
```

The samples have the same data cuts in their photometric properties. 

+ The samples are labelled 1 for QSO and 0 for PLO and are combined, shuffled and split into a training-set, cross-validation set and a test-set with 0.5, 0.25 and 0.25 of the data respectively.

## Method

## Code

## Results

- Bulleted
- List

1. Numbered
2. List
3. **special**

```markdown
code added here

# Header 1
## Header 2
### Header 3

**Bold** and _Italic_ and `Code` text

[Link](url) and ![Image](src)
```
