# PREVIEW INPUT FILE DENGAN JQUERY/JAVASCRIPT

Berikut merupakan preview gambar input file ketika diupload

```html
<!DOCTYPE html>
<html>
<head>
    <title>File Upload Preview</title>
    <style>
        #preview-container {
            text-align: center;
        }
        #preview-image {
            max-width: 100%;
            max-height: 300px;
            margin-top: 10px;
        }
    </style>
</head>
<body>
    <input type="file" id="file-input" accept="image/*">
    <div id="preview-container">
        <img id="preview-image" src="#" alt="Preview" style="display: none;">
    </div>
    <script src="script.js"></script>
</body>
</html>
```

Dengan Javascript

```js
document.getElementById('file-input').addEventListener('change', function(event) {
    var input = event.target;
    var previewImage = document.getElementById('preview-image');

    if (input.files && input.files[0]) {
        var reader = new FileReader();

        reader.onload = function(e) {
            previewImage.src = e.target.result;
            previewImage.style.display = 'block';
        };

        reader.readAsDataURL(input.files[0]);
    } else {
        previewImage.style.display = 'none';
        previewImage.src = '';
    }
});
```

Dengan Javascript (function dipisahkan)

```js
document.addEventListener('DOMContentLoaded', function() {
    var fileInput = document.getElementById('file-input');
    fileInput.addEventListener('change', function() {
        previewImage(this);
    });

    function previewImage(input) {
        var previewImage = document.getElementById('preview-image');

        if (input.files && input.files[0]) {
            var reader = new FileReader();

            reader.onload = function(e) {
                previewImage.src = e.target.result;
                previewImage.style.display = 'block';
            };

            reader.readAsDataURL(input.files[0]);
        } else {
            previewImage.style.display = 'none';
            previewImage.src = '';
        }
    }
});
```

Dengan JQuery

```js
$(document).ready(function() {
    $('#file-input').change(function() {
        var input = this;
        var previewImage = $('#preview-image');

        if (input.files && input.files[0]) {
            var reader = new FileReader();

            reader.onload = function(e) {
                previewImage.attr('src', e.target.result).css('display', 'block');
            };

            reader.readAsDataURL(input.files[0]);
        } else {
            previewImage.css('display', 'none').attr('src', '');
        }
    });
});
```

Dengan JQuery (function dipisahkan)
```js
$(document).ready(function() {
    $('#file-input').change(function() {
        previewImage(this);
    });
    
    function previewImage(input) {
        var previewImage = $('#preview-image');

        if (input.files && input.files[0]) {
            var reader = new FileReader();

            reader.onload = function(e) {
                previewImage.attr('src', e.target.result).css('display', 'block');
            };

            reader.readAsDataURL(input.files[0]);
        } else {
            previewImage.css('display', 'none').attr('src', '');
        }
    }
});
```

