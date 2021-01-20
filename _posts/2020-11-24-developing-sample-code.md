---
title: Complete the Essentials in the Code 
description: 15
---

<p><strong>1. Locate following line in *Presenter.kt classes </strong></p>
<pre><div id="copy-button10" class="copy-btn" title="Copy" onclick="copyCode(this.id)"></div><code> //TODO: Locate the TODO for adding a marker to Huawei Map with hMap instance<span class="pln">
</span></code></pre>
<p><strong>2. Adding a marker to Huawei Map</strong></p>
<pre><div id="copy-button11" class="copy-btn" title="Copy" onclick="copyCode(this.id)"></div><code>
override fun addMarkerToMap(lat: Double, lot: Double, title: String, snippet: String,descriptor:BitmapDescriptor?) {
    view.getMyMap()?.addMarker(
        MarkerOptions()
            .title(title)
            .snippet(snippet)
            .position(LatLng(lat, lot)).icon(descriptor)
    )
}
<span class="pln">
</span></code></pre>

<p><strong>3. Locate following line in *Presenter.kt classes</strong></p>
<pre><div id="copy-button12" class="copy-btn" title="Copy" onclick="copyCode(this.id)"></div><code> //TODO: Call the function onMapReady call back<span class="pln">
</span></code></pre>
<p><strong>4. Call the addMarkerToMap function onMapReady call back to add marker on certain position</strong></p>
<pre><div id="copy-button13" class="copy-btn" title="Copy" onclick="copyCode(this.id)"></div><code> addMarkerToMap(origin.lat, origin.lng, "Origin", "Start Point",null)
 addMarkerToMap(destination.lat, destination.lng, "Destination", "End Point",BitmapDescriptorFactory.fromResource(R.drawable.finish_flag_96))<span class="pln">
</span></code></pre>

<p><strong>5. Locate following line in *Presenter.kt classes</strong></p>
<pre><div id="copy-button19" class="copy-btn" title="Copy" onclick="copyCode(this.id)"></div><code> //TODO: Setting camera coordinates and moving camera on Huawei Map<span class="pln">
</span></code></pre>
<p><strong>6. Setting coordinates and moving camera</strong></p>
<pre><div id="copy-button20" class="copy-btn" title="Copy" onclick="copyCode(this.id)"></div><code>
val update = CameraUpdateFactory.newLatLngZoom(
    LatLng(
        origin.lat,
        origin.lng
    ), 11f
)
view.getMyMap()?.moveCamera(update);
<span class="pln">
</span></code></pre>

<p><strong>7. Locate following line in MyVolleyRequest.kt</strong></p>
<pre><div id="copy-button21" class="copy-btn" title="Copy" onclick="copyCode(this.id)"></div><code> //TODO: postRequest function to sending request and getting response <span class="pln">
</span></code></pre>
<p><strong>8. Create postRequest function to sending request and getting response</strong></p>
<pre><div id="copy-button22" class="copy-btn" title="Copy" onclick="copyCode(this.id)"></div><code>fun postRequest(dirReq: String, directionType: String,hMap:HuaweiMap,callBack:IVolley){
    val gson = Gson()
    // creating Error Listener
    val err =
        Response.ErrorListener {
                error -> Log.d("ERROR", "onErrorResponse: " + error.message) }
    // creating Response Listener
    val res: Response.Listener&lt;JSONObject&gt; =
        Response.Listener {
                response ->
          val mdirectionResponse =
                gson.fromJson(response.toString(), DirectionResponse::class.java)
            Log.d("mapres", "onResponse: $response")
            callBack.onSuccess(mdirectionResponse)
        }
    val jsonObjectRequest: JsonObjectRequest =
        object : JsonObjectRequest(Method.POST, getUrl(directionType), JSONObject(dirReq),
            res , err) {}
    addToRequestQueue(jsonObjectRequest)
}
<span class="pln">
</span></code></pre>
<span class="pln">
</span>
<ul>
  <li><strong>dirReq: </strong>It is used as direction request with origin and destination points.</li>
  <li><strong>directionType: </strong>It is used as direction type such as walking, driving and bicycling.</li>
  <li><strong>hMap: </strong>It is used as Huawei Map object.</li>
  <li><strong>callBack: </strong>It is used as IVolley object to getting response from fragment.</li>
</ul>
<span class="pln">
</span>
<p><strong>9. Locate following line in PolylineHelper.kt</strong></p>
<pre><div id="copy-button23" class="copy-btn" title="Copy" onclick="copyCode(this.id)"></div><code> //TODO: For drawing polyline on the map with response from postRequest<span class="pln">
</span></code></pre>
<p><strong>10. Drawing polyline on map.</strong></p>
<pre><div id="copy-button24" class="copy-btn" title="Copy" onclick="copyCode(this.id)"></div><code>
fun drawPolyline(directionResponse: DirectionResponse,hMap:HuaweiMap) {
    val pathList : ArrayList&lt;LatLng&gt; = arrayListOf()
    if (directionResponse.routes != null && directionResponse.routes!!.isNotEmpty()){
        val route = directionResponse.routes!![0]
        if (route.paths != null){
            for (i in route.paths!!){
                val path = i
                if (path.steps != null) {
                    for (j in path.steps!!){
                        if (j.polyline != null && j.polyline!!.isNotEmpty()){
                            for (k in j.polyline!!){
                                pathList.add(LatLng(k.lat!!,k.lng!!))
                            }
                        }
                    }
                }
            }
        }
    }
   mPolyline = hMap.addPolyline(
   PolylineOptions().addAll(pathList).color(Color.BLUE).width(4f))
}
<span class="pln"></span></code></pre>
<aside class="special">
	<p><strong>Note: In this codelab project we are getting the first route only to showing on the map</strong></p>
</aside>
<p><strong>11. Locate following line in *Presenter.kt classes </strong></p>
<pre><div id="copy-button25" class="copy-btn" title="Copy" onclick="copyCode(this.id)"></div><code> //TODO: For sending post request from fragment in onMapReady callback function <span class="pln">
</span></code></pre>
<p><strong>12. For sending post request</strong></p>
<pre><div id="copy-button26" class="copy-btn" title="Copy" onclick="copyCode(this.id)"></div><code>
context?.let {
    view.getMyMap().let { it1 ->
        if (it1 != null) {
            MyVolleyRequest.getInstance(it, this).postRequest(
                dirReq, DirectionType.WALK.type,
                it1, object : IVolley {
                    override fun onSuccess(directionResponse: DirectionResponse) {
                      mdirectionResponse = directionResponse
                      view.showAllSteps(mdirectionResponse!!)
                      PolylineHelper().drawPolyline(mdirectionResponse!!,view.getMyMap()!!)
                    }
                }
            )
        }
    }
}
<span class="pln">
</span></code></pre>

<ul>
  <li><strong>DirectionType.Walk.type: </strong>It is used as for getting “walking” value to sending request with walk rotation.</li>
  <li><strong>directionResponse: </strong>This response is returned when the request is sent. When using Volley Library for post request should send request and getting response with this.</li>
  <li><strong>directionAdapter: </strong>It is used as the  DirectionsAdapter instance for filling Recycler View with response of all steps.</li>
  <li><strong>callBack: </strong>It is used as IVolley object to getting response from fragment.</li>
</ul>

<p><strong>13. Locate following line in DirectionsAdapter.kt</strong></p>
<pre><div id="copy-button27" class="copy-btn" title="Copy" onclick="copyCode(this.id)"></div><code> //TODO: Creating DirectionsAdapter for showing route steps<span class="pln">
</span></code></pre>
<p><strong>14. Creating adapter for showing directions route steps</strong></p>
<pre><div id="copy-button28" class="copy-btn" title="Copy" onclick="copyCode(this.id)"></div><code>
override fun onBindViewHolder(holder: DirectionsHolder, position: Int) {
    val step: Step? = steps[position]
    if (step != null) {
        holder.actionDescription.text = step.instruction
        holder.actionDistanceTime.text = context.getString(R.string.distance_time,step.distanceText,step.durationText)
        when(step.action){
            DirectionActionType.STRAIGHT.type-> holder.actionImage.setImageDrawable(context.resources.getDrawable(R.drawable.ic_go,null))
…
<span class="pln">
</span></code></pre>

<ul>
  <li><strong>instruction: </strong>: It is used as step instruction. It is using when getting the map instructions to one place to another place.</li>
  <li><strong>distanceText: </strong>It used as showing the step distance. It is the distance between two steps with meter. For example 100 m etc.</li>
  <li><strong>durationText: </strong>It is used as showing the step duration. It is the duration between two steps with minute. For example 1 min etc.</li>
  <li><strong>stepAction: </strong>It is used as going to step with action. For example turn right, turn left etc.</li>
</ul>
</span></code></pre>
