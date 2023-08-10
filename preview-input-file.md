# PREVIEW INPUT FILE DENGAN JQUERY/JAVASCRIPT

## 1. Gambaran function umum dengan FileReader dan URL.createObjectURL

```js
var fileInput = document.getElementById('file-input');
var previewImage1 = document.getElementById('preview-image1');
var previewImage2 = document.getElementById('preview-image2');

fileInput.addEventListener('change', function() {
    previewWithObjectURL(fileInput, previewImage1);
    previewWithObjectURL2(fileInput, previewImage1);
    previewWithFileReader(fileInput, previewImage2);
});

function previewWithObjectURL(inputElement, previewElement) {
    if (inputElement.files && inputElement.files[0]) {
        var file = inputElement.files[0];
        previewElement.src = URL.createObjectURL(file);
        previewElement.style.display = 'block';
    } else {
        previewElement.style.display = 'none';
        previewElement.src = '';
    }
}

function previewWithObjectURL2(inputElement, previewElement) {
    if (inputElement.files && inputElement.files[0]) {
        var file = inputElement.files[0];
        var objectURL = URL.createObjectURL(file);
        
        previewElement.src = objectURL;
        previewElement.style.display = 'block';

        // Melepaskan URL objek setelah digunakan
        previewElement.onload = function() {
            URL.revokeObjectURL(objectURL);
        };
    } else {
        previewElement.style.display = 'none';
        previewElement.src = '';
    }
}

function previewWithFileReader(inputElement, previewElement) {
    if (inputElement.files && inputElement.files[0]) {
        var file = inputElement.files[0];
        var reader = new FileReader();

        reader.onload = function(e) {
            previewElement.src = e.target.result;
            previewElement.style.display = 'block';
        };

        reader.readAsDataURL(file);
    } else {
        previewElement.style.display = 'none';
        previewElement.src = '';
    }
}

```

<hr>

## 2. Dengan Studi kasus

html

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

### 2.1 Dengan FIleReader

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

### 2.2 Dengan URL.createObjectURL

javascript 

```js
document.addEventListener('DOMContentLoaded', function() {
    var fileInput = document.getElementById('file-input');
    var previewImage = document.getElementById('preview-image');
    var currentObjectURL = null;

    fileInput.addEventListener('change', function() {
        var file = fileInput.files[0];

        if (file) {
            if (currentObjectURL) {
                URL.revokeObjectURL(currentObjectURL); // Bebaskan URL objek sebelumnya
            }
            currentObjectURL = URL.createObjectURL(file);

            previewImage.src = currentObjectURL;
            previewImage.style.display = 'block';
        } else {
            previewImage.style.display = 'none';
            previewImage.src = '';
        }
    });
});
```

Javascript (dengan function terpisah)

```js
document.addEventListener('DOMContentLoaded', function() {
    var fileInput = document.getElementById('file-input');
    var previewImage = document.getElementById('preview-image');
    var currentObjectURL = null;

    function createPreview(file) {
        if (currentObjectURL) {
            URL.revokeObjectURL(currentObjectURL); // Bebaskan URL objek sebelumnya
        }
        currentObjectURL = URL.createObjectURL(file);

        previewImage.src = currentObjectURL;
        previewImage.style.display = 'block';
    }

    function clearPreview() {
        if (currentObjectURL) {
            URL.revokeObjectURL(currentObjectURL); // Bebaskan URL objek sebelumnya
        }
        previewImage.style.display = 'none';
        previewImage.src = '';
    }

    fileInput.addEventListener('change', function() {
        var file = fileInput.files[0];

        if (file) {
            createPreview(file);
        } else {
            clearPreview();
        }
    });
});

```

JQuery

```js
$(document).ready(function() {
    var fileInput = $('#file-input');
    var previewImage = $('#preview-image');

    fileInput.change(function() {
        var file = fileInput[0].files[0];

        if (file) {
            if (previewImage.attr('src')) {
                URL.revokeObjectURL(previewImage.attr('src')); // Bebaskan URL objek sebelumnya
            }
            previewImage.attr('src', URL.createObjectURL(file)).css('display', 'block');
        } else {
            previewImage.css('display', 'none').attr('src', '');
        }
    });
});
```

JQuery (dengan function terpisah)

```js
$(document).ready(function() {
    var fileInput = $('#file-input');
    var previewImage = $('#preview-image');
    var currentObjectURL = null;

    function createPreview(file) {
        if (currentObjectURL) {
            URL.revokeObjectURL(currentObjectURL); // Bebaskan URL objek sebelumnya
        }
        currentObjectURL = URL.createObjectURL(file);

        previewImage.attr('src', currentObjectURL).css('display', 'block');
    }

    function clearPreview() {
        if (currentObjectURL) {
            URL.revokeObjectURL(currentObjectURL); // Bebaskan URL objek sebelumnya
        }
        previewImage.css('display', 'none').attr('src', '');
    }

    fileInput.change(function() {
        var file = fileInput[0].files[0];

        if (file) {
            createPreview(file);
        } else {
            clearPreview();
        }
    });
});
```