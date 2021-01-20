---
title: Initialize Audio Kit SDK and Play Audio
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

<aside class="special">
	<p><strong>Note: AsyncTask is currently deprecated, please use other threading methods if you care about modernity in your application.</strong></p>
</aside>

<p><strong>5. Locate following line in AudioActivity.kt</strong></p>
<pre><div id="copy-button19" class="copy-btn" title="Copy" onclick="copyCode(this.id)"></div><code> //TODO: Implement onSongChange method of the listener<span class="pln">
</span></code></pre>
<p><strong>6. Implement the onSongChange method of the listener</strong></p>
<pre><div id="copy-button20" class="copy-btn" title="Copy" onclick="copyCode(this.id)"></div><code>
if(hwAudioPlayItem != null)
    setSongDetails(hwAudioPlayItem) //just update the song details everytime a song changes
if (mHwAudioPlayerManager!!.offsetTime != -1L && mHwAudioPlayerManager!!.duration != -1L) 
    updateSeekBar(mHwAudioPlayerManager!!.offsetTime, mHwAudioPlayerManager!!.duration) //also update seekbar if needed<span class="pln">
</span></code></pre>

<p><strong>7. Locate following line in AudioActivity.kt</strong></p>
<pre><div id="copy-button21" class="copy-btn" title="Copy" onclick="copyCode(this.id)"></div><code> //TODO: Implement onPlayProgress method of the listener<span class="pln">
</span></code></pre>
<p><strong>8. Implement onPlayProgress method of the listener</strong></p>
<pre><div id="copy-button22" class="copy-btn" title="Copy" onclick="copyCode(this.id)"></div><code>updateSeekBar(currentPosition, duration) //just update the seekbar as this method is called everytime the song progresses<span class="pln">
</span></code></pre>

<p><strong>9. Locate following line in AudioActivity.kt</strong></p>
<pre><div id="copy-button23" class="copy-btn" title="Copy" onclick="copyCode(this.id)"></div><code> //TODO: Implement the song progress on Audio Kit manager<span class="pln">
</span></code></pre>
<p><strong>10. Implement the song progress on Audio Kit manager</strong></p>
<pre><div id="copy-button24" class="copy-btn" title="Copy" onclick="copyCode(this.id)"></div><code>
if (mHwAudioPlayerManager != null && fromUser) {
    mHwAudioPlayerManager!!.seekTo(progress * 1000)
}<span class="pln"></span></code></pre>

<p><strong>11. Locate following line in PlaylistCreator.kt </strong></p>
<pre><div id="copy-button25" class="copy-btn" title="Copy" onclick="copyCode(this.id)"></div><code> //TODO: Retrieve local audio files<span class="pln">
</span></code></pre>
<p><strong>12. Retrieve local audio files that matches our criteria</strong></p>
<pre><div id="copy-button26" class="copy-btn" title="Copy" onclick="copyCode(this.id)"></div><code>
val songItem = HwAudioPlayItem()
songItem.audioTitle = cursor.getString(cursor.getColumnIndexOrThrow(MediaStore.Audio.Media.DISPLAY_NAME))
songItem.audioId = cursor.getLong(cursor.getColumnIndexOrThrow(MediaStore.Audio.Media._ID)).toString() + ""
songItem.filePath = path
songItem.setOnline(0) //0 means local
songItem.setIsOnline(0) //0 means local
if (Build.VERSION.SDK_INT >= Build.VERSION_CODES.Q) {
    songItem.duration = cursor.getLong(cursor.getColumnIndexOrThrow(MediaStore.Audio.Media.DURATION))
} else songItem.setDuration(0)
songItem.singer = cursor.getString(cursor.getColumnIndexOrThrow(MediaStore.Audio.Media.ARTIST))
playItemList.add(songItem)<span class="pln">
</span></code></pre>

<p><strong>13. Locate following line in PlaylistCreator.kt</strong></p>
<pre><div id="copy-button27" class="copy-btn" title="Copy" onclick="copyCode(this.id)"></div><code> //TODO: Retrieve an online playlist<span class="pln">
</span></code></pre>
<p><strong>14. Retrieve an online playlist of your selection</strong></p>
<pre><div id="copy-button28" class="copy-btn" title="Copy" onclick="copyCode(this.id)"></div><code>
val audioPlayItem1 = HwAudioPlayItem()
audioPlayItem1.audioId = "1000"
audioPlayItem1.singer = "Sample MP3 - Need lower bandwidth"
audioPlayItem1.onlinePath = "https://www.learningcontainer.com/wp-content/uploads/2020/02/Kalimba.mp3"
//Links are examples, you may use any links you want
//Please beware the copyright issues
audioPlayItem1.setOnline(1)
audioPlayItem1.audioTitle = "Kalimba - MP3"
playItemList.add(audioPlayItem1)
val audioPlayItem2 = HwAudioPlayItem()
audioPlayItem2.audioId = "1001"
audioPlayItem2.singer = "Sample FLAC - Need higher bandwidth"
audioPlayItem2.onlinePath = "https://www.learningcontainer.com/wp-content/uploads/2020/02/Sample-FLAC-File.flac"
audioPlayItem2.setOnline(1)
audioPlayItem2.audioTitle = "Kalimba - FLAC"
playItemList.add(audioPlayItem2)<span class="pln">
</span></code></pre>

<p><strong>15. Locate following line in AudioActivity.kt</strong></p>
<pre><div id="copy-button29" class="copy-btn" title="Copy" onclick="copyCode(this.id)"></div><code> //TODO: Implement the play button<span class="pln">
</span></code></pre>
<p><strong>16. Implement the play button both visually and functionally</strong></p>
<pre><div id="copy-button30" class="copy-btn" title="Copy" onclick="copyCode(this.id)"></div><code>
binding!!.playButtonImageView.setOnClickListener {
  if (binding!!.playButtonImageView.drawable.constantState == drawablePlay!!.constantState) {
      if (mHwAudioPlayerManager != null) {
          mHwAudioPlayerManager!!.play()
          binding!!.playButtonImageView.setImageDrawable(getDrawable(R.drawable.btn_playback_pause_normal))
          isReallyPlaying = true
      }
  } 
  else if (binding!!.playButtonImageView.drawable.constantState == drawablePause!!.constantState) {
      if (mHwAudioPlayerManager != null) {
          mHwAudioPlayerManager!!.pause()
          binding!!.playButtonImageView.setImageDrawable(getDrawable(R.drawable.btn_playback_play_normal))
          isReallyPlaying = false
      }
  }
    //Other button implementation are very similar, please check the rest of the code
    //Also, please notice that this outer method (handleOnClicks()) is called in onCreate() method
}<span class="pln">
</span></code></pre>
