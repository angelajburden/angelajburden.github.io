

{: id="data_banner"}
## Data

<p style="font-size:larger;color:black;margin-left: -20px;">This page describes the publicly available data set used in this project.</p>

<p style="font-size:larger;color:black;margin-left: -20px;">The data can be queried here <a href="http://skyserver.sdss.org/dr7/en/tools/search/sql.asp" style="color:DarkCyan;font-weight:bold;">SDSS DR7 DATA</a> </p>

![alt-text-1](/images/col_col.jpg "colours"){:height="56%" width="56%"}![alt-text-2](/images/hist_cats.jpg "colour errors"){:height="52%" width="52%"}

+ We choose 30000 known QSO (quasars) and their properties using the SQL commands

```
SELECT TOP 30000 s.z,s.zErr, s.specClass, p.psfMag_u, p.psfMag_g, p.psfMag_r, p.psfMag_i, p.psfMag_z, p.psfMagErr_u, p.psfMagErr_g, p.psfMagErr_r,p.psfMagErr_i,p.psfMagErr_z FROM BESTDR7 as s

JOIN BESTDR7 AS p ON s.bestObjID = p.objID

WHERE (s.specClass = dbo.fSpecClass('QSO') OR s.specClass = dbo.fSpecClass('HIZ_QSO')) AND s.zConf>0.35 AND s.z>0.5 AND s.z<5 AND p.psfMag_u>0 AND p.psfMag_u<26 AND p.psfMag_g>19 AND p.psfMag_g<25 AND p.psfMag_r>0 AND p.psfMag_r<24 AND p.psfMag_i>0 AND p.psfMag_i<24 AND p.psfMag_z>0 AND p.psfMag_z<24 AND p.psfMagErr_u>0 AND p.psfMagErr_g>0 AND p.psfMagErr_r>0 AND p.psfMagErr_i>0 AND p.psfMagErr_z>0

```

+ We choose 30000 other point-like objects (PLO) using the following commands


```
SELECT TOP 30000 s.z,s.zErr, s.specClass, p.psfMag_u,p.psfMag_g,p.psfMag_r,p.psfMag_i,p.psfMag_z, p.psfMagErr_u,p.psfMagErr_g,p.psfMagErr_r,p.psfMagErr_i,p.psfMagErr_z FROM BESTDR7 as s

JOIN BESTDR7 AS p ON s.bestObjID = p.objID

WHERE (s.specClass = dbo.fSpecClass('STAR')) AND p.psfMag_u>0 AND p.psfMag_u<26 AND p.psfMag_g>19 AND p.psfMag_g<25 AND p.psfMag_r>0 AND p.psfMag_r<24 AND p.psfMag_i>0 AND p.psfMag_i<24 AND p.psfMag_z>0 AND p.psfMag_z<24 AND p.psfMagErr_u>0 AND p.psfMagErr_g>0 AND p.psfMagErr_r>0 AND p.psfMagErr_i>0 AND p.psfMagErr_z>0 

```

The samples have the same data cuts in their photometric properties. 

+ The samples are labelled 1 for QSO and 0 for PLO and are combined, shuffled and split into a training-set, cross-validation set and a test-set with 0.5, 0.25 and 0.25 of the data respectively.

+ In astronomy we define the colours of an object to be the difference in the values between two bands for example 
psfMag_u - psfMag_g above is the u-g band. The u-g, g-r, r-i, i-z colours are added to the data.

+ In the neural network we initially use 10 input parameters (following https://arxiv.org/abs/0910.3770 ). They are the 4 colours defined above, the g magnitude and the 5 magnitude errors i.e. psfMagErr_z etc.

+ To show that the objects cannot be separated with data cuts we show plots of the input parameters of each object below using the training sample. Red objects are the QSO and blue are the PLO. 

### Training set input parameters
***
The 10 input parameters of each object are hown in the two plots at the top of the page. The QSOs are in red and the PLOs in blue. Clearly they cannot be identified by applying simple cuts to any of these attributes.



