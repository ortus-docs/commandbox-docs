# Allowed Static Files

The Undertow-based web server built into CommandBox will only serve up static files if they are in a list of valid extensions.  This is to prevent prying eyes from hitting files they shouldn't be able to access on your server. 

The current list of valid extensions is:

```text
3gp,3gpp,7z,ai,aif,aiff,asf,asx,atom,au,avi,bin,bmp,btm,cco,crt,css,csv,deb,der,dmg,
doc,docx,eot,eps,flv,font,gif,hqx,htc,htm,html,ico,img,ini,iso,jad,jng,jnlp,jpeg,jpg,
js,json,kar,kml,kmz,m3u8,m4a,m4v,map,mid,midi,mml,mng,mov,mp3,mp4,mpeg,mpeg4,mpg,msi,
msm,msp,ogg,otf,pdb,pdf,pem,pl,pm,png,ppt,pptx,prc,ps,psd,ra,rar,rpm,rss,rtf,run,sea,
shtml,sit,svg,svgz,swf,tar,tcl,tif,tiff,tk,ts,ttf,txt,wav,wbmp,webm,webp,wmf,wml,wmlc,
wmv,woff,woff2,xhtml,xls,xlsx,xml,xpi,xspf,zip,aifc,aac,apk,bak,bk,bz2,cdr,cmx,dat,
dtd,eml,fla,gz,gzip,ipa,ia,indd,hey,lz,maf,markdown,md,mkv,mp1,mp2,mpe,odt,ott,odg,
odf,ots,pps,pot,pmd,pub,raw,sdd,tsv,xcf,yml,yaml 
```

If you have a common static file you need to serve, you can add your own custom extensions to the list like so:

```bash
server set web.allowedExt=jar,exe,dll
```

