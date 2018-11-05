<center><h2>图片base64码转file对象</h2></center>

```javascript
dataURItoFile: function (dataURI, fileName) {
    var byteString = atob(dataURI.split(',')[1])
    var ab = new ArrayBuffer(byteString.length)
    var ia = new Uint8Array(ab)
    for (var i = 0; i < byteString.length; i++) {
        ia[i] = byteString.charCodeAt(i)
    }
    // return new Blob([ab], { type: 'image/jpeg' });
    return new File([ia], fileName, {type: 'image/jpeg', lastModified: Date.now()})
}
```

