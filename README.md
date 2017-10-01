# Android-Audio-Streaming-Code

private void playMusic(final String str)
  
  {
        
        if (mix != null)
        
        {
            
            mix.stop();
       
       }

//      mix = new MediaPlayer();

        mix =MediaPlayer.create(getApplicationContext(), getResources().getIdentifier(str,"raw",getPackageName()));
        
        mix.setOnPreparedListener(new MediaPlayer.OnPreparedListener() {
            @Override
            public void onPrepared(MediaPlayer mediaPlayer) {
                mix.start();
            }
        });
        
//////////////////////////////////////////////////////////////////////////////////////////////////////////////
        mix.setOnCompletionListener(new MediaPlayer.OnCompletionListener() {
            @Override
            public void onCompletion(MediaPlayer mediaPlayer) {
                if (music_index + 1 >= music_num ) {
                    final String str_song_name =  getSongNameFromModel(music_index_first);
                    changeImageIcon(music_index_first);
                    music_index = 0;
                    music_index_first = 1;
                    music_index += music_index_first;
                    playMusic(str_song_name);
                }
                else {
//                    UiUtils.showShortToast("next song is enable");
                    onClikNextButton();
                }
            }
        });

        final android.os.Handler mHandler = new android.os.Handler();
        this.runOnUiThread(new Runnable() {
            @Override
            public void run() {
                music_seekbar.setMax((int)mix.getDuration() / 1000);
                long mCurrentPostion = mix.getCurrentPosition();
                music_seekbar.setProgress((int)mCurrentPostion / 1000);

                timeCurrent.setText(milliSecondsToTimer(mCurrentPostion));
                timeEnd.setText(milliSecondsToTimer(mix.getDuration()));

                mHandler.postDelayed(this, 1000);
            }
        });
    }

    public  String milliSecondsToTimer(long milliseconds) {
        String finalTimerString = "";
        String secondsString = "";

        // Convert total duration into time
        int hours = (int) (milliseconds / (1000 * 60 * 60));
        int minutes = (int) (milliseconds % (1000 * 60 * 60)) / (1000 * 60);
        int seconds = (int) ((milliseconds % (1000 * 60 * 60)) % (1000 * 60) / 1000);
        // Add hours if there
        if (hours > 0) {
            finalTimerString = hours + ":";
        }

        // Prepending 0 to seconds if it is one digit
        if (seconds < 10) {
            secondsString = "0" + seconds;
        } else {
            secondsString = "" + seconds;
        }

        finalTimerString = finalTimerString + minutes + ":" + secondsString;

        // return timer string
        return finalTimerString;
    }
