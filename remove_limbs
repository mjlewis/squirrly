:Written by Matt Lewis; adapted from code of Jordan Bell and Glenn Orton

;This program allows you to identify a group of Saturn images which you would like to use. The images are grouped together according to date and wavelength in the
;observationgroups.pro file. An index to the path to each image is contained in the allcmaps.pro file. To select a group of images, uncomment the line where the variable 'set'
;is set equal to the group of images. (You can also directly set 'set' equal to an array of image indices as defined in allcmaps.pro, they are just grouped by observation for  
;convenience.) The program creates a pointer array of the images in 'set', and creates another pointer array for the corresponding mu (cosine of the emission angle) files 
;(it won't work if the mu file hasn't been created). As it is creating the pointer array of the images, it converts the radiance numbers to brightness temperature. 
;This doesn't work if the images have not all been properly calibrated. Make sure the images are calibrated correctly (this should be apparent in the window which displays 
;the fits later on).
;
;The next step removes the edges of the images which are dimmed by diffraction. This is determined by the 'percmu' variable which you set and will vary from date to date and
;especially from instrument to instrument. A percmu of 45 means that data corresponding to mu from 0.45 to 1.0 is kept while the rest is cut off. I usually find that a percmu of 45
;is ok for MIRSI data, while a percmu of 35 or even lower is ok for VISIR and COMICS. Always take this on a case-by-case basis though depending on how the fits look in the fitting 
;window. You will see where the data begins to drop off sharply at the edges. This is the part we want to throw away, probably with a little extra for good measure. There are some 
;dates where we have to use higher percmus because of bad seeing or some other effect. Always take as much as you can, but not more or you will get extra waves on the edges which 
;shouldn't be there.
;
;Now comes the fitting. We are going to fit the image data with the mu data. For Saturn we are assuming that the atmosphere is well mixed along each line of latitude so that the 
;emission depends only on mu (cosine of emission angle). We will use either a first- or second-order polynomial in mu to fit the data. There are a few options here. You can pick a
;fit that fits the entire planet with the same polynomial coefficients, but this is usually not good for much. You may want to do this if you have a bright beacon or something. 
;Enter 0 for fitdegree if you want to do this. The other fits fit one latitude at a time, which removes the brightness difference from one latitude to the next. Setting fitdegree 
;to 1 will give a first-order polynomial fit, setting it to 2 will give a second-order fit. There are some images which appear to be slanted, meaning they are brighter on one side 
;than the other. I don't know why this happens. It could be that someone assigned incorrect geometry to the image when they made the cylindrical map and the mu map, but I tried 
;redoing the geometry on one of them and it was still slanted. It may be that the images were not correctly divided by a flat or that there was some other problem. Anyway, in order 
;to still use these images, I made two more fits, which should be the same as the first- and second-order options, but they also allow for a slant to be added to the fit. Enter 
;either -1 or -2 to use these fits. They seem to work well for those images which are slanted. Usually they can also work for the normal flat images as well. The fit just sets the 
;slant very close to zero. When choosing a fit, be extremely careful! No fit will be perfect, but use the one that introduces as few extra features as possible. If the whole planet 
;isn't slanted, it is possible that there is a real slant in the data for a particular latitude and using a slantfit would throw away good information. There are also times when 
;using a first-order fit will introduce waves on the edges or using a second-order fit will introduce extra waves in the middle from the wiggles of the fitting function. Be careful 
;to check each fit for proper behavior. You may need to increase percmu to resolve some of the problem.
;
;At this point, the program will output a plot of both the data and the fits along one line of latitude (specifed as 'lat'). Here is where you check to make sure the fit you chose 
;is working well. Make sure that mu isn't too low. (You shouldn't see drop-off at the edges of the data. Try a low mu once to see what I mean.) Also make sure that the data curves l
;line up at approximately the same brightness temperature level. Some variation is ok, because we're just looking at the waves, but too much means that something is wrong with 
;calibration for one or more images.
;
;
;
;
;
;
;
;
;
;
;
;
;
;
;
;
;
;
;
;
;
;
;
;allcmaps.pro
;observationdates.pro
;brightemp.pro
;muchop.pro
;image_1fit.pro
;image_2fit.pro
;image_2allfit.pro
;image_1slantfit.pro
;image_2slantfit.pro
;sigmoid_map_stitch.pro
;
;
;
;
;
;
;
pro remove_limbs
cmap=strarr(2500)
loadmu=strarr(2500)
@allcmaps
@observationgroups
device,decomposed=0
loadct,3

;set=[m_031215_1220,m_031216_1220,m_031217_1220,m_031218_1220];done,45,-1,0.25,212,242
;set=[m_031215_1720,m_031216_1720,m_031217_1720];done
;set=[659,660];
;set=[m_041027_1720,m_041028_1720];done
;set=[m_041216_1220,m_041217_1220];m_041215_1220,,m_041218_1220];close to other obs,noise;done,47,-1,0.25,215,245
set=[m_041215_1720,m_041216_1720,m_041218_1720];done
;set=[m_050130_1220,m_050131_1220];,m_050201_1220;done,San Francisco, CA48,-1,0.25,215,245
;set=[m_050129_1720,m_050130_1720,m_050131_1720];done
;set=[m_050225_1220,m_050226_1220,m_050227_1220];m_050224_1220,;done,47,-1,0.25,205,245
;set=[m_050225_1720,m_050226_1720,m_050228_1720];done
;set=[m_050502_1220,m_050506_1220,m_050507_1220];,m_050504_1220];done,47,-1,0.25,208,245
;set=[m_050505_1720,m_050506_1720,m_050507_1720];done
;set=[m_050523_1220,m_050524_1220];,m_050525_1220;done,45,1,0.15,208,245
;set=[m_050523_1720,m_050525_1720];done
;set=[m_050611_1220,m_050614_1220];,m_050612_1220;done,45,-1,0.25,208,247
;set=[m_050611_1720,m_050614_1720];done
;set=[m_051024_1220];done,47,-1,0.25,205,253
;set=[m_051019_1720,m_051022_1720,m_051024_1720];done
;set=[m_051120_1220,m_051121_1220];done,47,-1,0.25,205,230
;set=[m_051120_1720,m_051121_1720];done
;set=[m_051216_1220,m_051217_1220];done,47,-2,0.25,210,247
;set=[m_051216_1720,m_051217_1720];done
;set=[m_060129_1220,m_060130_1220];done,47,-1,0.25,205,253
;set=[m_060130_1720];done
;set=[m_060414_1220,m_060415_1220,m_060416_1220];done,47,-1,0.25,210,250
;set=[m_060414_1720,m_060415_1720];done
;set=[m_060524_1220,m_060525_1220,m_060526_1220];done,45,-2,0.3,208,253
;set=[m_060524_1720,m_060525_1720];done
;set=[m_060620_1220,m_060621_1220,m_060622_1220];done,47,-2,0.25,208,253
;set=[m_060620_1720,m_060621_1720,m_060622_1720];done
;set=[m_070505_1220];done,47,-1,208,253
;set=[m_070505_1720];done
;set=[m_070611_1220];done,50,-1,208,253
;set=[m_070611_1720];done
;set=[m_071005_1220];done,60,-1,190,215
;set=[m_071005_1720];done
;set=[m_080117_1220];done,60,-1,184,209
;set=[m_080117_1720];done
;set=[m_080320_1720];done
;set=[m_080711_1220];noisy
;set=[m_080711_1720];done
;set=[m_081105_1220];done,60,-1,175,200
;set=[m_081104_1720,m_081105_1720];done
;set=[m_081223_1220,m_081222_1220];,satellite;done,50,-1,0.3,170,195
;set=[m_081222_1720,m_081223_1720];done
;set=[m_090502_1219,m_090503_1219];done,60,-1,0.3,175,200
;set=[m_090502_1720];done
;set=[m_091222_1219,m_091223_1219,m_091224_1219];done,55,-1,0.13,160,185
;set=[m_091222_1720,m_091223_1720,m_091224_1720];done
;set=[m_100109_1219];done,47,1,0.25,155,180
;set=[m_100109_1720];done
;set=[m_100330_1219,m_100331_1219,m_100402_1219];done,55,-1,0.25,165,190
;set=[m_100330_1720,m_100331_1720,m_100402_1720];done
;set=[m_100623_1219,m_100624_1219,m_100625_1219];done,60,-1,0.15,165,190
;set=[m_100623_1720,m_100624_1720,m_100625_1720];done
;set=[m_100630_1219,m_100701_1219];done,53,-1,0.25,163,188
;set=[m_100630_1720,m_100701_1720];done
;set=[m_110325_1219,m_110326_1219];done,53,1,0.25,150,175
;set=[m_110325_1720];done
;set=[m_100623_1840,m_100624_1840,m_100625_1840]
;set=[m_031215_770,m_031217_770,m_031218_770]
;set=[m_041027_770,m_041028_770]
;set=[m_041215_770,m_041216_770,m_041217_770]
;set=[m_050130_770,m_050131_770];bad? seems pretty bad.
;set=[v_100318_1227,v_100321_1227];done,25,-2,0.25,162,183
;set=[c_130501_1250];done,35,-1,0.25,122,160, note that a teeny bit of the rings gets picked up at -30 lat
;set=[c_090113_1250,c_090114_1250];done,35,-2,0.15,175,188
;set=[c_050430_1250];done,30,-2,205,248
;set=[v_080412_1227,v_080415_1227];done,29,-2,0.35,185,220
;set=[v_080521_1227,v_080522_1227];done,27,-2,0.25,182,217
;set=[v_080610_1227];done,29,-2,0.25,181,217
;set=[v_110325_1227,v_110326_1227];done,29,-1,0.25,145,175

;set=[2147,2148,2154]
;set=[1842,1870]
;set=[m_100624_2480,m_100625_2480]
;set=[c_050430_2450]
;set=[m_100630_2480,m_100701_2480]
;set=[c_071212_2450]
;set=[c_080123_2450]
;set=[c_090114_2450]
;set=[v_100318_1765,v_100321_1765]
;set=[c_071212_780]
;set=[c_080123_780]
;set=[c_090113_780,c_090114_780]
;set=[c_130501_780]
;set=[c_050430_870]
;set=[c_050524_870]
;set=[c_071212_870]
;set=[r_000221_2082]
;set=[k_040204_800]
;set=[k_040204_1875]
;set=[k_040204_2310]
;set=[k_040204_2450]
;set=[k_040204_1765]
;set=[r_001230_785]
;set=[r_020207_1793,,]
;set=[r_020207_2082]
;set=[r_020207_2420,r_020211_2420]
;set=[r_020207_785,r_020208_785,r_020210_785,r_020211_785]
;set=[r_020207_1220]	
;set=[r_020211_785]
;set=[r_020209_1793]
;set=[r_020208_1793]
;set=[r_020210_1793]
;set=[r_020209_2082]
;set=[r_001230_1724]
;set=[r_020209_1867]
;set=[r_030228_1724]
;set=[r_030223_1867,r_030227_1867,r_030228_1867]
;set=[r_030223_2082,r_030228_2082]
;set=[r_030224_785,r_030225_785,r_030227_785,r_030228_785,r_030301_785]
;set=[r_030223_1220,r_030224_1220,r_030225_1220,r_030227_1220,r_030301_1220]
;set=[r_020207_2420]
;set=[v_100318_790,v_100321_790]
;set=[m_100630_2480,m_100701_2480]
;set=[m_100630_770,m_100701_770]
;set=[m_060414_770]
;set=[m_071005_770]
;set=[m_071005_770]
;set=[r_981004_790]
;set=[r_020208_1793]
;set=[m_110426_1720]
;set=[m_110802_1720]
;set=[c_130501_1765]
;set=[r_960627_1793]
;set=[c_080123_1250]
;set=[r_980720_1220]

date='2004_Dec_15-18'
instrument='MIRSI'
lambda='17.20' ;this must be set correctly to give proper brightness temperature

save=1
lat=-30           ;this is the latitude measured from the equator along which data will be sampled for plotting
percmu=53		;this is the range of mu that we show, e.g. 70 means mu ranges from 0.7 to 1.0
 ;the degree of the polynomial used in the latitude by latitude fit (12 to 3, 0 for a planet-wide 2nd-order fit, or -1 for a slanted 1st order -2 for slanted 2nd order) 
s=0.25             ;this is the stiffness of the sigmoid stitch. 0.2 is usually pretty good. high values lead to seam down middle, low values lead to a seam on each side.
ringbottom=175
ringtop=188
cutoff=30
;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;
n=2*(lat+90)      ;this is the index of the row of pixels corresponding to the latitude above (invert the two equations if you want to specify n instead of lat)
wavelength=strtrim(lambda,2)


for i=0,n_elements(cmap)-1 do begin
if cmap[i] ne '' then begin
place=strpos(cmap[i],'cmap')
muhead=strmid(cmap[i],0,place)
mutail=strmid(cmap[i],place+4)
loadmu[i]=muhead+'mu'+mutail
endif
endfor

;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;this will create a pointer array of the images
imageset = PtrArr(n_elements(set))
for i=0,n_elements(set)-1 do begin
imageset[i] = Ptr_New(brightemp(congrid(readfits(cmap[set(i)],h),720,360,/cubic),lambda))
;imageset[i] = Ptr_New(congrid(readfits(cmap[set(i)],h),720,360,/cubic))
endfor


;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;this will create a pointer array of the mu images
muset = PtrArr(n_elements(set))
for i=0,n_elements(set)-1 do begin
muset[i] = Ptr_New(congrid(readfits(loadmu[set(i)],hm),720,360,/cubic))
endfor


;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;this removes (sets to zero) parts of the images that are outside the specified mu range
cutimageset = PtrArr(n_elements(set));
for i=0,n_elements(set)-1 do begin
cutimageset[i] = Ptr_New(muchop((*imageset[i]),(*muset[i]),percmu))
(*cutimageset[i])[where(*cutimageset[i] lt cutoff)]=0.0
endfor


;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;this removes (sets to zero) parts of the mu images that are outside the specified range
;cutmuset = PtrArr(n_elements(set));
;for i=0,n_elements(set)-1 do begin
;cutmuset[i] = Ptr_New(muchop((*muset[i]),(*muset[i]),percmu))
;endfor


;for i=0,n_elements(set)-1 do begin
;;*cutimageset[i]=pixel_interp(*cutimageset[i],*muset[i],-1)
;firstfit=image_1slantfit((*cutimageset[i]),(*cutmuset[i]))
;(*cutimageset[i])[where(*cutmuset[i] gt 0.0 and *cutimageset[i] eq 0.0)]=firstfit[where(*cutmuset[i] gt 0.0 and *cutimageset[i] eq 0.0)]
;endfor

;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;this creates a least-squares fit of the image data for use as a 'zonal mean' which accounts for limb effects
fitimageset0 = PtrArr(n_elements(set))
fitimageset1 = PtrArr(n_elements(set))
fitimageset2 = PtrArr(n_elements(set));
for i=0,n_elements(set)-1 do begin


fitimageset2[i] = Ptr_New(image_2fit((*cutimageset[i]),(*muset[i])))

fitimageset0[i] = Ptr_New(image_2allfit((*cutimageset[i]),(*muset[i])))
fitimageset1[i] = Ptr_New(image_1slantfit((*cutimageset[i]),(*muset[i])))

endfor


window,0,title='Data and Fits'
plot,(*imageset[0])(*,n),color=0,background=16777215,thick=2,yrange=[100,120]
oplot,(*fitimageset1[0])(*,n),color=145, thick=5
;oplot,(*fitimageset0[0])(*,n),color=245, thick=5
;oplot,(*fitimageset2[0])(*,n),color=45, thick=5


for i=1,n_elements(set)-1 do begin
oplot,(*imageset[i])(*,n),color=0,thick=2
oplot,(*fitimageset1[i])(*,n),color=145, thick=5
;oplot,(*fitimageset0[i])(*,n),color=205, thick=4
;oplot,(*fitimageset2[i])(*,n),color=95, thick=5
endfor
if save then write_gif,date+'_'+wavelength+'_'+instrument+'_'+'_model_and_data_at_'+strtrim(lat,2)+'_degrees_lat.gif',tvrd()



;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;this subtracts the fit from each image to get the residuals
normimageset = PtrArr(n_elements(set));
normimagesetmean = PtrArr(n_elements(set));
normimageset1 = PtrArr(n_elements(set));
normimageset0 = PtrArr(n_elements(set));
normimageset2 = PtrArr(n_elements(set));
for i=0,n_elements(set)-1 do begin
normimageset[i] = Ptr_New(*cutimageset[i])
normimagesetmean[i]=Ptr_New(*cutimageset[i])
normimageset0[i] = Ptr_New(*cutimageset[i])
normimageset1[i] = Ptr_New(*cutimageset[i])
normimageset2[i] = Ptr_New(*cutimageset[i])

(*normimageset[i])[where(*cutimageset[i])] = ((*cutimageset[i])[where(*cutimageset[i])])-((*fitimageset1[i])[where(*cutimageset[i])])

;(*normimagesetmean[i])[where(*cutimageset[i])] = ((*cutimageset[i])[where(*cutimageset[i])])-mean;;(((*cutimageset[i])[where(*cutimageset[i])]))

;(*normimageset0[i])[where(*cutimageset[i])] = ((*cutimageset[i])[where(*cutimageset[i])])-((*fitimageset0[i])[where(*cutimageset[i])])

;(*normimageset1[i])[where(*cutimageset[i])] = ((*cutimageset[i])[where(*cutimageset[i])])-((*fitimageset1[i])[where(*cutimageset[i])])

;(*normimageset2[i])[where(*cutimageset[i])] = ((*cutimageset[i])[where(*cutimageset[i])])-((*fitimageset2[i])[where(*cutimageset[i])])


endfor




window,1,XSIZE=720,YSIZE=360,title='Residuals'
display=*normimageset[0]
;displaymean=*normimagesetmean[0]
;display0=*normimageset0[0]
;display1=*normimageset1[0]
;display2=*normimageset2[0]
for i=1,n_elements(set)-1 do begin
display=sigmoid_map_stitch(display,*normimageset[i],s)
;displaymean=sigmoid_map_stitch(displaymean,*normimagesetmean[i],s)
;display0=sigmoid_map_stitch(display0,*normimageset0[i],s)
;display1=sigmoid_map_stitch(display1,*normimageset1[i],s)
;display2=sigmoid_map_stitch(display2,*normimageset2[i],s)
endfor


tvscl,display[0:719,0:359]+113.0,xsize=72,ysize=36;[720:1439,*]
if save then write_gif, date+'_'+wavelength+'_'+instrument+'_'+'residuals.gif',tvrd()
if save then drm_writefits,date+'_'+wavelength+'_'+instrument+'_'+'residuals.gz',display


window,2,XSIZE=720,YSIZE=360,title='Limb Effects Removed'
flatimage=display
for i=0,359 do begin
meantemp=0.0	
	for j=0,n_elements(set)-1 do begin
	meantemp=meantemp+mean((*cutimageset[j])[where((*cutimageset[j])[*,i]),i])
	endfor
meantemp=meantemp/(1.0*n_elements(set))
flatimage[where(flatimage[*,i]),i]=flatimage[where(flatimage[*,i]),i]+meantemp
endfor
status=despike(flatimage,badpix,flatimage,nsigma=7)
tvscl,flatimage[0:719,0:359],xsize=72,ysize=36;[720:1439,*]

if save then write_gif, date+'_'+wavelength+'_'+instrument+'.gif',tvrd()
if save then drm_writefits, date+'_'+wavelength+'_'+instrument+'.gz',flatimage


title3='Residuals along '+strtrim(lat,2)+' degrees lat.'
window,3,XSIZE=720,YSIZE=360,title=title3
plot,display[*,n],color=0,background=16777215, xtitle='System-III West Longitude',ytitle='Residuals', thick=3, title='Residuals Along -30 Degree Latitude'
;oplot,display1[*,n],color=130,thick=3
;oplot,display0[*,n],color=230,thick=3
;oplot,display2[*,n],color=170,thick=3




window,4,XSIZE=720,YSIZE=360,title='Rings Removed Residuals'
display3=display
if save eq 0 then begin
display3[*,ringtop]=0.0
display3[*,ringbottom]=0.0
display3[*,n]=0.0
tvscl,display3
endif
if save then begin
display3=display
display3[*,ringbottom:ringtop]=0.0
tvscl,display3
write_gif, date+'_'+wavelength+'_'+instrument+'_'+'residuals_rbo.gif',tvrd()
drm_writefits,date+'_'+wavelength+'_'+instrument+'_'+'residuals_rbo.gz',display3
window,5,XSIZE=720,YSIZE=360,title='Rings Removed Limb Effects Removed'

display4=flatimage
display4[*,ringbottom:ringtop]=0.0
tvscl,display4
write_gif, date+'_'+wavelength+'_'+instrument+'_'+'rbo.gif',tvrd()
drm_writefits,date+'_'+wavelength+'_'+instrument+'_'+'rbo.gz',display4


endif











heap_gc
end
