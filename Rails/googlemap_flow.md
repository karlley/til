# googlemap_flow

GoogleMap の描画までの処理の流れ

## 処理の流れ

1. controller `@destination` を取得
2. controller `@destination` から`Gmaps4rails` を使用して座標, infowindowの値を`@marker` に保存
3. view `@marker`からマップを表示

model で`geocoder` が座標を取得する対象(`address_keyword`) を指定

```Ruby
# model

  # geocoder で経度, 緯度を取得する
  geocoded_by :address_keyword
  after_validation :geocode
```

```Ruby
# controller

  def show
    @destination = Destination.find(params[:id])
    # GoogleMap 表示用のマーカーを作成
    @marker = Gmaps4rails.build_markers(@destination) do |destination, marker|
      marker.lat(destination.latitude)
      marker.lng(destination.longitude)
      marker.infowindow render_to_string(partial: "destinations/map_infowindow", locals: { destination: destination })
    end
  end
```

```Ruby
# view

<div id="map"></div>

<script type="text/javascript">
  handler = Gmaps.build('Google');
  handler.buildMap({ provider: { scrollwheel: false }, internal: {id: 'map'}}, function(){
    markers = handler.addMarkers(<%= raw @marker.to_json %>);
    handler.bounds.extendWith(markers);
    handler.fitMapToBounds();
    handler.getMap().setZoom(16);
  });
</script>
```
