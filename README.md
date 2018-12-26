# banner-module

## Prerequisites
<ul>
	<li><a target="_blank" href="https://getbootstrap.com/">Boostrap4</a></li>
</ul>

## Step 1: Add the Form
 - banner-form.tpl

Create a calendar for the Banner and upload the following form.

```
<div class="panel-group">
  <div class="panel panel-default">
    <div class="panel-heading">
      <h4 class="panel-title">
        <a data-toggle="collapse" href="#collapseStatus" aria-expanded="true">Post Status<span class="toggle" aria-hidden="true"></span></a>
      </h4>
    </div>
    <div id="collapseStatus" class="panel-collapse collapse in">
      <div class="panel-body">
        <div class="row">
          <div class="col-md-3">
            <h2><label class="label-control" for="post_status">Post Status</label></h2>
            <select class="form-control" type="text" name="post_status">
              <option value="Draft">Draft</option>
              <option value="Published">Published</option>
            </select>
          </div>
        </div>
      </div>
    </div>
  </div>
</div>

<div class="panel-group">
  <div class="panel panel-default">
    <div class="panel-heading">
      <h4 class="panel-title">
        <a data-toggle="collapse" href="#collapseLinks" aria-expanded="true">Banner Info <span class="toggle" aria-hidden="true"></span></a>
      </h4>
    </div>
    <div id="collapseLinks" class="panel-collapse collapse in">
      <div class="panel-body">
        <div class="row">
          <div class="col-md-6">
            <h2><label class="label-control" for="banner_link">Link</label></h2>
            <input type="text" class="form-control" name="banner_link" id="banner_link">
          </div>

          <div class="col-md-6">
            <h2><label class="label-control" for="banner_new_window">Open in New Window</label></h2>
            <input type="checkbox" name="url_new_window" id="banner_new_window" value="1">
          </div>
          </div>
          <div class="row">
          <div class="col-md-6" id="listingImage">
                <h2><label class="label-control" for="banner_image">Banner Image</label></h2>
                <p class="subText"><strong>Sizes:</strong><br /> Square (Right Sidebar) - 296X296<br />Rectangle: (Bottom of Page) 727 Width X 113 Height</p>
                <input type="file" class="file_upload" name="banner_image" id="banner_image" required>
            </div>
        </div>
      </div>
    </div>
  </div>
</div>
<div class="panel-group">
  <div class="panel panel-default">
    <div class="panel-heading">
      <h4 class="panel-title">
        <a data-toggle="collapse" href="#collapseBannerDisplay"  aria-expanded="true">Banner Display <span class="toggle" aria-hidden="true"></span></a>
      </h4>
    </div>
    <div id="collapseBannerDisplay" class="panel-collapse collapse in">
      <div class="panel-body">
        <div class="row">
        <div class="col-md-3">
            <h2><label class="label-control" for="banner_size">Ad Size</label></h2>
            <select class="form-control" type="text" name="banner_size">
              <option value="square">Square</option>
              <option value="rectangle">Rectangle</option>
            </select>
          </div>
          <div class="col-md-3">
            <h2><label class="label-control" for="banner_all_pages">Show on All Pages</label></h2>
            <select class="form-control" type="text" name="banner_all_pages">
              <option value="no">No</option>
              <option value="yes">Yes</option>
            </select>
          </div>
        </div>
      </div>
    </div>
  </div>
</div>



<div class="panel-group">
  <div class="panel panel-default">
    <div class="panel-heading">
      <h4 class="panel-title">
        <a data-toggle="collapse" href="#collapseBannerDisplay">Advanced <span class="toggle" aria-hidden="true"></span></a>
      </h4>
    </div>
    <div id="collapseBannerDisplay" class="panel-collapse collapse">
      <div class="panel-body">
        <div class="row">
          <div class="col-md-12">
            <h2><label class="label-control" for="post_javascript">Custom JavaScript</label></h2>
            <p class="subText">(Optional) Use the following textbox to embed any custom JavaScript including tracking pixels and Google Analytics scripts. Be sure to open your JavaScript with a &lt;script&gt; tag and close everything with a &lt;/script&gt; tag.</p>
            <textarea class="form-control" name="post_javascript" id="post_javascript"></textarea>
          </div>
        </div>
      </div>
    </div>
  </div>
</div>


```

## Step 2: Add the Repeater
 - banner-repeater.tpl

Add the following repeater shortcode. This should be placed within a &lt;ul&gt; tag in the navigation, and be shown when hovered. 

```
[repeater id="11" limit="0,1" where="post_status='Published']
  <div>
  	<a href="{{banner_link}}">
    	[cond type="is" subject="{{banner_size}}" value="square"]
    		<img class="w-200p h-200p cover" src="[get_asset_file_url id={{banner_image}}]">
        [/cond]
        [cond type="is_not" subject="{{banner_size}}" value="square"]
            <img class="w-100" src="[get_asset_file_url id={{banner_image}}]">
        [/cond]
    </a>
  </div>
[/repeater]

```

## Step 3: Add the SCSS/CSS
  - /_/scss/banner.scss
  
```
.w-200p {
    width: 200px;
}

.h-200p {
    height: 200px;
}

.cover {
    object-fit: cover;
}

```

## Step 4: Add the JS
  - /_/js/banner.js
  
  
  ```
  
  window.onload = function() {

    // Detect IE
    function detectIE() {
      var ua = window.navigator.userAgent;
  
      var msie = ua.indexOf('MSIE ');
      if (msie > 0) {
        // IE 10 or older => return version number
        return parseInt(ua.substring(msie + 5, ua.indexOf('.', msie)), 10);
      }
  
      var trident = ua.indexOf('Trident/');
      if (trident > 0) {
        // IE 11 => return version number
        var rv = ua.indexOf('rv:');
        return parseInt(ua.substring(rv + 3, ua.indexOf('.', rv)), 10);
      }
  
      var edge = ua.indexOf('Edge/');
      if (edge > 0) {
        // Edge (IE 12+) => return version number
        return parseInt(ua.substring(edge + 5, ua.indexOf('.', edge)), 10);
      }
  
      // other browser
      return false;
    }
  
  
    var isIE = detectIE();
    
    if (isIE != false) {

      var obCoverImgs = document.querySelectorAll('.cover');
      var imgLength;
      imgLength = obCoverImgs.length;
      var thisParent;
      var newDiv;
      var thisSRC;

      for (var i = 0; i < imgLength; i++) {

        thisSRC = obCoverImgs[i].src;
        thisParent = obCoverImgs[i].parentNode;

        if (thisSRC) {

          obCoverImgs[i].className += " hidden";

          newDiv = document.createElement("div");
          newDiv.className = "image-hero-container";

          newDiv.style.backgroundImage = "url('" + thisSRC + "')";
          
          // If image is using height classes, take those for the containing div. These will override the fallback of height on page load.
          for (var j = 0; j < obCoverImgs[i].classList.length; j++) {
            if (obCoverImgs[i].classList[j].match(/^h-/)) {
              newDiv.className += " " + obCoverImgs[i].classList[j];
            }
          }
          newDiv.style.height = obCoverImgs[i].clientHeight + "px";


          newDiv.appendChild(obCoverImgs[i]);
          thisParent.insertBefore(newDiv, thisParent.firstChild);

        }
      }
    }
};

  
  
  ```
