# Rails jQuery-fileUpload-with-Drag&ProgressBar

基本上這多屬於前端的工




## 1. 允許拖拉上傳是js產生的行為，用jquery-file-upload這個library去產生

```rb
# gemfile

# ...
gem "jquery-fileupload-rails"
```

------------------------


```coffee
# app/assets/javascripts/_plugins.js.coffee 

# ...
#= require jquery-fileupload/basic
# ...
```

----


```slim
/ app/views/albums/show.html.slim

= form_for [@album, Photo.new ] do |f|
  = f.label :image, "上傳圖檔(亦可拖拉檔案至頁面):", class: "image"
  = f.file_field :image, multiple: true, class: "image-input"

/ ...

= content_for :handwrite_js
 coffee:
   jQuery ->
     $('#new_photo').fileupload()
```

上面用form_for幫忙產生了表單，自幹表單要注意上傳檔案該有的參數

jQuery用id抓到這個form，執行了fileupload()，整個頁面是可以拖拉上傳的區域(拜jquery-fileupload所賜)

拉了檔案到頁面上就會自動送出，照著form_helper去打controller的create action

[API文件](https://github.com/blueimp/jQuery-File-Upload/wiki/API)





## 2. controller接到params然後create，接著response to something

```rb
# app/controllers/album_photos_controller.rb

  def create
    @album = current_user.albums.find(params[:album_id])
    photo_params[:image].each do |image|
      @photo = @album.photos.create(image: image, description: photo_params[:description])
    end
    # redirect_to album_path(@album)
  end
```

把redirect去掉因為拖拉上傳就是要方便，要方便就是要自動，要自動就是懶得按送出，懶得按送出就是要自動上傳，  

自動上傳就是要自動新增，要自動新增那就最好不要refresh，那就用js在資料建好之後動態加上html，

所以要先把一個照片的顯示區塊做成partial，好讓之後新的資料有適當的html區塊疊加上來。

```slim
/ app/views/albums/_photo.html.slim

.col-sm-6.col-md-4
  .thumbnail
    - if photo.image.present?
      a href="#{ album_photo_path(album, photo)}"
        img src="#{photo.image.thumb}"
    - else
      a href="#{ album_photo_path(album, photo)}"
        = image_placeholder width: 200, height: 150, text: 'no image'
    .caption
      h4 相片敘述：#{photo.description}
      - if photo.album.user == current_user
          td = link_to '編輯', edit_album_photo_path(album, photo), class: 'btn btn-info'
          td = link_to '刪除', album_photo_path(album, photo), method: :delete,  data: { confirm: 'Are you sure?', :'confirm-button-text' => 'Im sure', :'cancel-button-text' => 'No way', :'confirm-button-color' => '#66CD00', :'sweet-alert-type' => 'info'}, class: "btn btn-danger"
```
照片區塊的partial





## 3. 接到新的record param後帶進一個partial用js去append前端頁面

```erb
<!--app/views/album_photos/create.js.erb-->

<% if @photo.new_record? %>
  alert("Failed to upload painting: <%= j @photo.errors.full_messages.join(', ').html_safe %>");
<% else %>
  $("#photo-container").append("<%= j render partial: 'albums/photo', locals: { photo: @photo, album: @album} %>");
<% end %>
```
因為這個file名稱跟上面controller是一樣的關係，然後又沒有redirect的方向，

所以controller除了執行命令之外會自動來render這個templete

new_record?在validation沒過的時候會是true因為沒有save成功，然後就看要用什麼方式來顯示錯誤訊息

alert很討厭，看看是不是之後弄成flash message





## 4. 顯示進度條，也是用library提供的api,分析完資料之後把進度條append到指定的區域

```coffee
# app/assets/javascripts/_plugins.js.coffee

#= require jquery-fileupload/vendor/tmpl
```
再加上這個library，用來把變數放進去產生方便操作的html template





## 5. 把一個div當做進度條，然後把寬度被動態更新從0改到100%～

```slim
/ app/views/albums/show.html.slim

script id="template-upload" type="text/x-tmpl"
  .upload
    | {%=o.name%}
    .progress
      .bar style="width: 0%"


/ ...

coffee:
  jQuery ->
    $('#new_photo').fileupload
      dataType: "script"
      add: (e, data) ->
        data.context = $(tmpl("template-upload", data.files[0]))
        $('#new_photo').append(data.context)
        data.submit()
      progress: (e, data) ->
        if data.context
          progress = parseInt(data.loaded / data.total * 100, 10)
          data.context.find('.bar').css('width', progress + '%')
```

上方的部份是安插想要顯示進度條的位置，裡面帶一個可以接js變數的區塊用來顯示進度條的檔案名稱

下方script的add則是在執行append這個動作,progress則是在動態算出檔案上傳的百分比，

然後找出要當成進度條的div去動態改變寬度，圖形化檔案上傳進度。





## 6. 把礙眼的進度條弄掉

```slim
/  app/views/albums/show.html.slim

stop: (e, data) ->
  $('.upload').fadeOut('slow')
```
>   接在progress後面，檔案都上傳，進度都跑完，把進度條慢慢fadeout




## 7.前端js驗證

```coffee
# app/views/albums/show.html.slim

coffee:
  jQuery ->
    $('#new_photo').fileupload
      dataType: "script"
      add: (e, data) ->
        types = /(\.|\/)(gif|jpe?g|png)$/i
        file = data.files[0]
        if types.test(file.type) || types.test(file.name)
          data.context = $(tmpl("template-upload", file))
          $('#new_photo').append(data.context)
          data.submit()
        else
          alert("#{file.name} is not a gif, jpeg, or png image file")
      progress: (e, data) ->
        if data.context
          progress = parseInt(data.loaded / data.total * 100, 10)
          data.context.find('.progress-bar').css('width', progress + '%')
      stop: (e, data) ->
        $('.upload').fadeOut('slow')

```

在拖拉的時候就直接檢查是不是合法的檔案格式，不然就跳alert

## 8.打包好做成class

```coffee
# app/assets/javascripts/classes/_fileipload.js.coffee

class Wuxian.Fileupload
  jQuery ->
    $('#new_photo').fileupload
      dataType: "script"
      add: (e, data) ->
        types = /(\.|\/)(gif|jpe?g|png)$/i
        file = data.files[0]
        if types.test(file.type) || types.test(file.name)
          data.context = $(tmpl("template-upload", file))
          $('#new_photo').append(data.context)
          data.submit()
        else
          alert("#{file.name} is not a gif, jpeg, or png image file")
      progress: (e, data) ->
        if data.context
          progress = parseInt(data.loaded / data.total * 100, 10)
          data.context.find('.progress-bar').css('width', progress + '%')
      stop: (e, data) ->
        $('.upload').fadeOut('slow')

```


```slim
/  app/views/albums/show.html.slim

= content_for :handwrite_js
  coffee:
    $(document).ready( new Wuxian.Fileupload )
```
整理好以後方便使用～dry


----------


參考資料:  
https://github.com/railscasts/381-jquery-file-upload
https://www.youtube.com/watch?v=x23aIQPa-DY
https://github.com/tors/jquery-fileupload-rails
http://railscasts.com/episodes/381-jquery-file-upload?view=comments
https://github.com/blueimp/jQuery-File-Upload/wiki/API
