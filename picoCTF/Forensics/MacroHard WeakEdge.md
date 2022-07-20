# **_MacroHard WeakEdge_**
## Description
> I've hidden a flag in this file. Can you find it? `Forensics is fun.pptm`

## Solution
Use `binwalk` to check for hidden files, there is alot files in the `pptm` document, extract them using `-e` 
`ls -R` to check all file extracted
```
❯ ls -R _Forensics\ is\ fun.pptm.extracted
'_Forensics is fun.pptm.extracted':
 0.zip  '[Content_Types].xml'   docProps   ppt   _rels

'_Forensics is fun.pptm.extracted/docProps':
app.xml  core.xml  thumbnail.jpeg

'_Forensics is fun.pptm.extracted/ppt':
presentation.xml  _rels         slideMasters  tableStyles.xml  vbaProject.bin
presProps.xml     slideLayouts  slides        theme            viewProps.xml

'_Forensics is fun.pptm.extracted/ppt/_rels':
presentation.xml.rels

'_Forensics is fun.pptm.extracted/ppt/slideLayouts':
_rels              slideLayout11.xml  slideLayout2.xml  slideLayout4.xml  slideLayout6.xml  slideLayout8.xml
slideLayout10.xml  slideLayout1.xml   slideLayout3.xml  slideLayout5.xml  slideLayout7.xml  slideLayout9.xml

'_Forensics is fun.pptm.extracted/ppt/slideLayouts/_rels':
slideLayout10.xml.rels  slideLayout2.xml.rels  slideLayout5.xml.rels  slideLayout8.xml.rels
slideLayout11.xml.rels  slideLayout3.xml.rels  slideLayout6.xml.rels  slideLayout9.xml.rels
slideLayout1.xml.rels   slideLayout4.xml.rels  slideLayout7.xml.rels

'_Forensics is fun.pptm.extracted/ppt/slideMasters':
hidden  _rels  slideMaster1.xml

'_Forensics is fun.pptm.extracted/ppt/slideMasters/_rels':
slideMaster1.xml.rels

'_Forensics is fun.pptm.extracted/ppt/slides':
_rels        slide16.xml  slide22.xml  slide29.xml  slide35.xml  slide41.xml  slide48.xml  slide54.xml  slide7.xml
slide10.xml  slide17.xml  slide23.xml  slide2.xml   slide36.xml  slide42.xml  slide49.xml  slide55.xml  slide8.xml
slide11.xml  slide18.xml  slide24.xml  slide30.xml  slide37.xml  slide43.xml  slide4.xml   slide56.xml  slide9.xml
slide12.xml  slide19.xml  slide25.xml  slide31.xml  slide38.xml  slide44.xml  slide50.xml  slide57.xml
slide13.xml  slide1.xml   slide26.xml  slide32.xml  slide39.xml  slide45.xml  slide51.xml  slide58.xml
slide14.xml  slide20.xml  slide27.xml  slide33.xml  slide3.xml   slide46.xml  slide52.xml  slide5.xml
slide15.xml  slide21.xml  slide28.xml  slide34.xml  slide40.xml  slide47.xml  slide53.xml  slide6.xml

'_Forensics is fun.pptm.extracted/ppt/slides/_rels':
slide10.xml.rels  slide1.xml.rels   slide29.xml.rels  slide38.xml.rels  slide47.xml.rels  slide56.xml.rels
slide11.xml.rels  slide20.xml.rels  slide2.xml.rels   slide39.xml.rels  slide48.xml.rels  slide57.xml.rels
slide12.xml.rels  slide21.xml.rels  slide30.xml.rels  slide3.xml.rels   slide49.xml.rels  slide58.xml.rels
slide13.xml.rels  slide22.xml.rels  slide31.xml.rels  slide40.xml.rels  slide4.xml.rels   slide5.xml.rels
slide14.xml.rels  slide23.xml.rels  slide32.xml.rels  slide41.xml.rels  slide50.xml.rels  slide6.xml.rels
slide15.xml.rels  slide24.xml.rels  slide33.xml.rels  slide42.xml.rels  slide51.xml.rels  slide7.xml.rels
slide16.xml.rels  slide25.xml.rels  slide34.xml.rels  slide43.xml.rels  slide52.xml.rels  slide8.xml.rels
slide17.xml.rels  slide26.xml.rels  slide35.xml.rels  slide44.xml.rels  slide53.xml.rels  slide9.xml.rels
slide18.xml.rels  slide27.xml.rels  slide36.xml.rels  slide45.xml.rels  slide54.xml.rels
slide19.xml.rels  slide28.xml.rels  slide37.xml.rels  slide46.xml.rels  slide55.xml.rels

'_Forensics is fun.pptm.extracted/ppt/theme':
theme1.xml

'_Forensics is fun.pptm.extracted/_rels':
```
Notice there is a `hidden` file under `slideMasters` directory
>'_Forensics is fun.pptm.extracted/ppt/slideMasters':
hidden  

It is a text file
```console
❯ file hidden
hidden: ASCII text, with no line terminators
```
Lets check its context
```console
❯ cat hidden
Z m x h Z z o g c G l j b 0 N U R n t E M W R f d V 9 r b j B 3 X 3 B w d H N f c l 9 6 M X A 1 f Q
```
Looks like its a `base64` encoded text, remove the spaces with `sed` and decode it to get the flag
```console
❯ echo "Z m x h Z z o g c G l j b 0 N U R n t E M W R f d V 9 r b j B 3 X 3 B w d H N f c l 9 6 M X A 1 f Q" | sed "s/ //g"| base64 -d
flag: picoCTF{D1d_u_kn0w_ppts_r_z1p5}
```

