# BUTON ACTION UNTUK BOOTSTRAP 4 dan 5 

```html
<div class="btn-group">
  <button type="button" class="btn btn-info dropdown-toggle" data-toggle="dropdown" data-boundary="viewport" aria-haspopup="true" aria-expanded="false">
    Aksi
  </button>
  <div class="dropdown-menu">
    <a class="dropdown-item" href="#"><i class="fas fa-eye"></i> Detail</a>
    <a class="dropdown-item" href="#"><i class="fas fa-edit"></i> Ubah</a>
    <a class="dropdown-item" href="#"><i class="fas fa-trash-alt"></i> Hapus</a>
  </div>
</div>
``


jika berada di dalam Datatable, tambahkan script berikut agar dropdown nya tampil keluar dari table

```js
<script>
  $(document).on('shown.bs.dropdown', '.table', function(e) {
    var $container = $(e.target);

    var $dropdown = $container.find('.dropdown-menu');
    if ($dropdown.length) {
        $container.data('dropdown-menu', $dropdown);
    } else {
        $dropdown = $container.data('dropdown-menu');
    }

    $dropdown.css('top', ($container.offset().top + $container.outerHeight()) + 'px');
    $dropdown.css('left', $container.offset().left + 'px');
    $dropdown.css('position', 'absolute');
    $dropdown.css('display', 'block');
    $dropdown.appendTo('body');
  });

  $(document).on('hide.bs.dropdown', '.table-responsive', function(e) {
    $(e.target).data('dropdown-menu').css('display', 'none');
  });
</script>
```