# Nasamat
# this is the Main of my App , you can find it on google paly store Nasamat Android App developed in Java on Android Studio that displays nearby places with details (distance, rating, is open, phone, photos ) using threads,  location,Rest API with Json, design patterns and other libraries for UI/UX: Picasso, Flowing-Drawer, Swipy


#https://play.google.com/store/apps/details?id=com.amr.nearbyopen






#package com.amr.nearbyopen;
#import android.Manifest;
#import android.annotation.SuppressLint;
#import android.app.Activity;
#import android.app.AlertDialog;
#import android.app.Dialog;
#import android.content.Context;
#import android.content.DialogInterface;
#import android.content.Intent;
#import android.content.pm.PackageManager;
#import android.content.res.Configuration;
#import android.graphics.Color;
#import android.graphics.Rect;
#//import android.inputmethodservice.Keyboard;
#import android.location.Location;
#import android.net.ConnectivityManager;
#import android.net.NetworkInfo;
#import android.net.Uri;
#import android.os.Bundle;
#import android.os.CountDownTimer;
#//import android.provider.Settings;
#import android.support.annotation.NonNull;
#import android.support.annotation.Nullable;
#import android.support.constraint.ConstraintLayout;
#import android.support.design.widget.NavigationView;
#import android.support.v4.app.ActivityCompat;
#//import android.support.v4.view.GravityCompat;
#import android.support.v4.view.MotionEventCompat;
#import android.support.v4.widget.DrawerLayout;
#import android.support.v4.widget.SwipeRefreshLayout;
#import android.support.v7.widget.LinearLayoutManager;
#import android.support.v7.widget.RecyclerView;
#import android.text.Editable;
#import android.text.InputType;
#import android.text.TextWatcher;
#import android.util.Log;
#import android.view.Gravity;
#import android.view.LayoutInflater;
#import android.view.MenuItem;
#import android.view.MotionEvent;
#import android.view.View;
#//import android.view.ViewGroup;
#import android.view.inputmethod.EditorInfo;
#import android.view.inputmethod.InputMethodManager;
#//import android.widget.Button;
#import android.widget.EditText;
#import android.widget.ImageView;
#//import android.widget.MediaController;
#import android.widget.LinearLayout;
#import android.widget.TextView;
#import android.widget.Toast;
#import android.widget.VideoView;
#import com.amr.nearbyopen.PlacesNeraby.PalacAdapter;
#import com.amr.nearbyopen.PlacesNeraby.Place;
#import com.amr.nearbyopen.PlacesNeraby.PlacesDataSource;
#import com.crashlytics.android.Crashlytics;
#import com.google.android.gms.common.ConnectionResult;
#import com.google.android.gms.common.api.GoogleApiClient;
#import com.google.android.gms.location.LocationListener;
#import com.google.android.gms.location.LocationRequest;
#import com.google.android.gms.location.LocationServices;
#import com.jaredrummler.materialspinner.MaterialSpinner;
#import com.mxn.soul.flowingdrawer_core.ElasticDrawer;
#import com.mxn.soul.flowingdrawer_core.FlowingDrawer;
#import com.xw.repo.BubbleSeekBar;
#import io.fabric.sdk.android.Fabric;
#import java.util.ArrayList;
#import java.util.Random;
#import static android.view.inputmethod.EditorInfo.IME_ACTION_DONE;
#import static com.amr.nearbyopen.PlacesNeraby.PalacAdapter.MY_PERMISSIONS_REQUEST_READ_CONTACTS;
#import static com.google.android.gms.location.LocationServices.*;
#public class MainActivity extends Activity implements PlacesDataSource.OnPlacesArrivedListener, GoogleApiClient.ConnectionCallbacks, #GoogleApiClient.OnConnectionFailedListener, LocationListener, NavigationView.OnNavigationItemSelectedListener {
#private static final int GAS_STATION_COVER_PHOTO =6 ;
#ImageView mImageMain;
#private final static int AROUND_ME_COVER_PHOTO = 0;
#private final static int RESTURANT_COVER_PHOTO = 2;
#private final static int COFE_SHOP_COVER_PHOTO = 3;
#private final static int SUPERMARKET_COVER_PHOTO = 4;
#private final static int BAKERY_COVER_PHOTO = 1;
#private final static int PARK_COVER_PHOTO = 5;
#public static int LAST_COVER_PHOTO = 1;
#EditText mSearch;
#SwipeRefreshLayout mSwipeContainer;
#public static ObjectsHolder ob = new ObjectsHolder();
#ImageView mClose;
#private final String LOG_TAG = "LaurenceTestApp";
#private GoogleApiClient mGoogleApiClient;
#private LocationRequest mLocationRequest;
#private ImageView mBtnHome;
#private ImageView mBtnFile;
#private ImageView mCategory;
#private ImageView mBtnClos;
#private VideoView mVideoview;
#private TextView mErrorText;
#private Rect mRect = new Rect();
#MaterialSpinner mSpinnerAroundMe;
#MaterialSpinner mSpinnerResturant;
#MaterialSpinner mSpinner3;
#MaterialSpinner mSpinnerSuperMarket;
#MaterialSpinner mSpinnerBakery;
#LinearLayout mLinear_around_me;
#LinearLayout mLinear_resturant;
#LinearLayout mLinear_supermarket;
#LinearLayout mLinear_bakery;
#String hours = "";
#FlowingDrawer mDrawer;
#AlertDialog.Builder builder;
#int b = 0;
#Dialog dialog;
#ImageView mTopList;
#String str = "";
#ImageView img, contact;
#//ImageView mmm;
#final Context context = this;
#public static int a = 0;
#RecyclerView rvPalaces;
#private static final int RC_REQUEST_LOCATION = 1;
#private Location location;

    public String getStr() {

        return ob.getD();
    }

    public void setStr(String str) {

        ob.setD(str);
        this.str = str;
    }


    public void onPlacesArrived(@Nullable final ArrayList<Place> places, @Nullable final Exception e) {
        //The places are received in a background thread.

        //runOnUIThread(Runnable runnable)
        //run on UI thread... a method that runs code on the ui (main) thread.

        runOnUiThread(new Runnable() {

            @Override
            public void run() {

                //code that runs on the UI (Main) Thread...!

                if (places != null) {
                    updateUI(places);


                } else if (e != null) {
                    Toast.makeText(MainActivity.this, e.getLocalizedMessage(), Toast.LENGTH_LONG).show();
                    e.printStackTrace();

                }

            }
        });


    }

    @Override
    public boolean dispatchTouchEvent(MotionEvent ev) {
        final int action = MotionEventCompat.getActionMasked(ev);
/*************
 * this code is perfect for hide keybored
 *
 *
 *
 * ********/
        // mSpinnerAroundMe.setBackgroundColor(Color.parseColor("#00000000"));

        int[] location = new int[2];
        mSearch.getLocationOnScreen(location);
        mRect.left = location[0];
        mRect.top = location[1];
        mRect.right = location[0] + mSearch.getWidth();
        mRect.bottom = location[1] + mSearch.getHeight();

        int x = (int) ev.getX();
        int y = (int) ev.getY();

        if (action == MotionEvent.ACTION_DOWN && !mRect.contains(x, y)) {
            InputMethodManager input = (InputMethodManager) getSystemService(Context.INPUT_METHOD_SERVICE);
            input.hideSoftInputFromWindow(mSearch.getWindowToken(), 0);
        }
        mSearch.setInputType(InputType.TYPE_NULL);

        return super.dispatchTouchEvent(ev);
    }

    private void toastMessageWel() {
        Toast toast = new Toast(this);
        toast.setDuration(Toast.LENGTH_LONG);
        toast.setGravity(Gravity.CENTER | Gravity.TOP | Gravity.FILL, 0, 0);
        LayoutInflater ly = getLayoutInflater();


        View view = ly.inflate(R.layout.toast_welcome, findViewById(R.id.imageView6));
        TextView textView = view.findViewById(R.id.textView9);
        TextView textView2 = view.findViewById(R.id.textView12);
       /* mVideoview = (VideoView) view.findViewById(R.id.imageView6);
    Uri uri = Uri.parse("android.resource://" + getPackageName() + "/" + R.raw.zzzx);
    mVideoview.setVideoURI(uri);
    mVideoview.start();*/
        toast.setView(view); //this to show the video on toast
        toast.show();
        new CountDownTimer(30, 30) {
            //this is for short time
            public void onTick(long millisUntilFinished) {
                toast.show();
            }

            public void onFinish() {
                toast.show();
            }

        }.start();
    }



    @SuppressLint("ResourceAsColor")
    private void updateUI(ArrayList<Place> palaces) {
        //1) find the recycler by id.
        // synchronized (this) {
       /* int forPhoto = getChangeSpinnerPhoto();
        if (forPhoto > 5)
            forPhoto = 20;
        getRandomImageInMAin(forPhoto);*/
        ConnectivityManager conMgr = (ConnectivityManager) getSystemService(Context.CONNECTIVITY_SERVICE);
        NetworkInfo netInfo = conMgr.getActiveNetworkInfo();//this for internr conection
        if (netInfo == null) {
            //Toast toast =
            Toast.makeText(context, context.getString(R.string.internetn) + " ", Toast.LENGTH_SHORT).show();
            /*View bg = toast.getView();
            bg.setBackgroundColor(R.color.colorAccent);
            toast.setGravity(Gravity.CENTER, 0, 0);
            toast.show();*/
        }
        rvPalaces = findViewById(R.id.rvPalaces);
        rvPalaces.setNestedScrollingEnabled(true);
        mSwipeContainer.setNestedScrollingEnabled(true);
        mErrorText = findViewById(R.id.error_text);
        //mmm = findViewById(R.id.mmm);


        // rvPalaces.bringToFront();
        mSwipeContainer.bringToFront();

        //the adapter takes Palaces and provides Views for the palaces.
        //2) PlacesAdapter adapter = new palaces adapter (palaces, context)
        PalacAdapter adapter = new PalacAdapter(palaces, MainActivity.this);

        //3) recycler -> take the adapter.
        rvPalaces.setAdapter(adapter);
        // mmm.setBackgroundResource(R.drawable.bcg);


        //4)
        rvPalaces.setLayoutManager(new LinearLayoutManager(null));
        if (a == 0) {//in the first time "welcome to app"
            //mmm.setVisibility(View.INVISIBLE);
            //mmm.setBackgroundResource(R.drawable.well);

        } else {
            ob.setCount(1);
            // mVideoview.setVisibility(View.INVISIBLE);
            // mBtnClos.setVisibility(View.INVISIBLE);
            rvPalaces.setVisibility(View.VISIBLE);

            if (palaces.size() == 0)//when not found result
            {
                //mmm.setVisibility(View.VISIBLE);
                // mmm.setBackgroundResource(R.drawable.sadd);
                mErrorText.setVisibility(View.VISIBLE);
                rvPalaces.setLayoutParams(new ConstraintLayout.LayoutParams(ConstraintLayout.LayoutParams.MATCH_PARENT, ConstraintLayout.LayoutParams.MATCH_PARENT));


            } else //when have a results
            {
                //  mmm.setVisibility(View.VISIBLE);
                //  mmm.setBackgroundColor(Color.WHITE);
                mErrorText.setVisibility(View.INVISIBLE);
            }


        }
        //
        a++;
        //}
        if (palaces.size() != 0) {
            //mmm.setVisibility(View.VISIBLE);
            // mmm.setBackgroundColor(Color.WHITE);


        }


    }

    private void getRandomImageInMAin(int spinner) {
        mImageMain = findViewById(R.id.image_top_spinners);
        Random rand = new Random();
        if (/*getStr().equals(getString(R.string.coffee_house)) ||*/ spinner == COFE_SHOP_COVER_PHOTO) {
            int num = rand.nextInt(3 - 0 + 1) + 0;
            if (num == 0)
                mImageMain.setImageDrawable(getResources().getDrawable(R.drawable.hc1));
            if (num == 1)
                mImageMain.setImageDrawable(getResources().getDrawable(R.drawable.hc2));
            if (num == 2)
                mImageMain.setImageDrawable(getResources().getDrawable(R.drawable.hc3));


        } else if (spinner == PARK_COVER_PHOTO) {
            int num = rand.nextInt(2 - 0 + 1) + 0;
            if (num == 0)
                mImageMain.setImageDrawable(getResources().getDrawable(R.drawable.pk2));
            if (num == 1)
                mImageMain.setImageDrawable(getResources().getDrawable(R.drawable.pk3));


        } else if (spinner == BAKERY_COVER_PHOTO) {
            int num = rand.nextInt(2 - 0 + 1) + 0;
            if (num == 0)
                mImageMain.setImageDrawable(getResources().getDrawable(R.drawable.bk1));
            if (num == 1)
                mImageMain.setImageDrawable(getResources().getDrawable(R.drawable.bk3));


        } else if (spinner == RESTURANT_COVER_PHOTO) {

            int num = rand.nextInt(5 - 0 + 1) + 0;
            if (num == 0)
                mImageMain.setImageDrawable(getResources().getDrawable(R.drawable.rs3));
            if (num == 1)
                mImageMain.setImageDrawable(getResources().getDrawable(R.drawable.rs4));
            if (num == 2)
                mImageMain.setImageDrawable(getResources().getDrawable(R.drawable.rs6));
            if (num == 3)
                mImageMain.setImageDrawable(getResources().getDrawable(R.drawable.rs7));
            if (num == 4)
                mImageMain.setImageDrawable(getResources().getDrawable(R.drawable.rs8));

        } else if (spinner == SUPERMARKET_COVER_PHOTO) {
            // int num = rand.nextInt(2 - 0 + 1) + 0;
           /* if (num == 0)
                mImageMain.setImageDrawable(getResources().getDrawable(R.drawable.sm2));
          */ // if (num == 1)
            mImageMain.setImageDrawable(getResources().getDrawable(R.drawable.sm3));

        } else if (spinner == AROUND_ME_COVER_PHOTO) {
            int num = rand.nextInt(4 - 0 + 1) + 0;
            if (num == 0)
                mImageMain.setImageDrawable(getResources().getDrawable(R.drawable.ss2));
            if (num == 1)
                mImageMain.setImageDrawable(getResources().getDrawable(R.drawable.ss));

        } else if (spinner == GAS_STATION_COVER_PHOTO) {
            mImageMain.setImageDrawable(getResources().getDrawable(R.drawable.gas_image));

        }

    }

    @Override
    public void onConfigurationChanged(Configuration newConfig) {
        super.onConfigurationChanged(newConfig);

// when open app  the keypoared display up
        //this func hide the keypored
    }

    @Override
    public void onRequestPermissionsResult(int requestCode, @NonNull String[] permissions, @NonNull int[] grantResults) {
        super.onRequestPermissionsResult(requestCode, permissions, grantResults);
        if (requestCode == RC_REQUEST_LOCATION && grantResults[0] == PackageManager.PERMISSION_GRANTED) {
            buildGoogleApiClient();
        }
        if (requestCode == MY_PERMISSIONS_REQUEST_READ_CONTACTS) {
            if (grantResults[0] == PackageManager.PERMISSION_GRANTED) {
                //Toast.makeText(context, "yeeeeees", Toast.LENGTH_LONG).show();
                phoneCall();


            } else {
                //  Toast.makeText(context, "fffffffff", Toast.LENGTH_LONG).show();
                // permission denied, boo! Disable the
                // functionality that depends on this permission.
            }
            return;
        }

    }

    private void phoneCall() {
        if (ActivityCompat.checkSelfPermission(context,
                Manifest.permission.CALL_PHONE) == PackageManager.PERMISSION_GRANTED) {
            Intent callIntent = new Intent(Intent.ACTION_CALL);
            callIntent.setData(Uri.parse(ob.getNumber()));
            context.startActivity(callIntent);
        } else {
            // Toast.makeText(context, "You don't assign permission.", Toast.LENGTH_SHORT).show();
        }
    }

    public Location getLocation() {
        return MainActivity.this.location;
    }

    public void setLocation(Location location) {
        MainActivity.this.location.setLongitude(location.getLongitude());
        MainActivity.this.location.setLatitude(location.getLatitude());
    }

    private void setSelected(int i) {
        if (i != 3 && i != 4 && i != 5) {
            mBtnHome.setSelected(false);
            mBtnFile.setSelected(false);
        }
        mCategory.setPressed(false);
        // img.setPressed(false);
        mTopList.setPressed(false);


        switch (i) {
            case 1:
                mBtnHome.setSelected(true);
                break;
            case 2:

                mBtnFile.setSelected(true);
                break;
            case 3:
                mCategory.setPressed(true);
                break;
            case 4:
                img.setPressed(true);
                break;
            case 5:
                mTopList.setPressed(true);
                break;

        }
    }

    public void setHours(String hours) {
        this.hours = hours;
    }

    public String getHours() {
        return hours;
    }

    private void doYourUpdate() { //this for the referesh when slide down
        // TODO implement a refresh
        PlacesDataSource.getPlaces(context, MainActivity.this, location, getStr(), getHours(),getRadios());
        mSwipeContainer.setRefreshing(false); // Disables the refresh icon

    }

    public void setRadios(int radios) {
        this.radios = radios;
    }

    int radios=0;

    private int getRadios() {

        return radios;
    }
    NavigationView navigationView;// = findViewById(R.id.nav_view);

    private void drawerFunc() {
        /*NavigationView*/ navigationView = findViewById(R.id.nav_view);
        navigationView.setNavigationItemSelectedListener(this);
        navigationView.setCheckedItem(R.id.around_me_drawer);
        DrawerLayout drawer = findViewById(R.id.drawer_layout);
        mDrawer = (FlowingDrawer) findViewById(R.id.drawerlayout);
        mDrawer.setTouchMode(ElasticDrawer.TOUCH_MODE_BEZEL);
        //https://github.com/mxn21/FlowingDrawer
        mDrawer.setOnDrawerStateChangeListener(new ElasticDrawer.OnDrawerStateChangeListener() {
            @Override
            public void onDrawerStateChange(int oldState, int newState) {
                if (newState == ElasticDrawer.STATE_CLOSED) {
                    Log.i("MainActivity", "Drawer STATE_CLOSED");
                }
            }

            @Override
            public void onDrawerSlide(float openRatio, int offsetPixels) {
                Log.i("MainActivity", "openRatio=" + openRatio + " ,offsetPixels=" + offsetPixels);
            }
        });


    }

    @Override
    public void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        Fabric.with(this, new Crashlytics());
        toastMessageWel();

        try {
            buildGoogleApiClient();
            setContentView(R.layout.activity_main);
            radiosKm();    drawerFunc();
            topListPlaces();
            mSwipeContainer();
            mBtnHome();
            mBtnFile();
            mCategory();
            mSearch();
            mClose();
            setSelected(1);
        } catch (Exception e) {
        }
    }
    BubbleSeekBar mBbubbleSeekBar;
    TextView mRadios;
    private void radiosKm() {
       try {
           mBbubbleSeekBar = findViewById(R.id.aa);
           final boolean[] radiosFlag = {true};
           mRadios = findViewById(R.id.radios);
           mRadios.setOnClickListener(new View.OnClickListener() {
               @Override
               public void onClick(View v) {
                   radiosFlag[0] = !radiosFlag[0];
                   if (radiosFlag[0] == false) {
                       mBbubbleSeekBar.setVisibility(View.INVISIBLE);
                       mRadios.setSelected(false);
                   } else {
                       mBbubbleSeekBar.setVisibility(View.VISIBLE);
                       mRadios.setSelected(true);

                   }
               }
           });
           mBbubbleSeekBar.getConfigBuilder().secondTrackColor(getResources().getColor(R.color.rec1))
                   .min((float) 0.0)
                   .max(50)
                   .progress(10)
                   .sectionCount(5)
                   .showSectionText()
                   .sectionTextSize(18)
                   .showThumbText()
                   .thumbTextSize(18)
                   .bubbleTextSize(18)
                   .showSectionMark()
                   .seekBySection()
                   .autoAdjustSectionMark()
                   .sectionTextPosition(BubbleSeekBar.TextPosition.BELOW_SECTION_MARK);
           mBbubbleSeekBar.setOnProgressChangedListener(new BubbleSeekBar.OnProgressChangedListener() {
               @Override
               public void onProgressChanged(BubbleSeekBar bubbleSeekBar, int progress, float progressFloat) {
                   //Toast.makeText(bubbleSeekBar.getContext(), ""+progress, Toast.LENGTH_SHORT).show();
               }

               @Override
               public void getProgressOnActionUp(BubbleSeekBar bubbleSeekBar, int progress, float progressFloat) {
                   //Toast.makeText(bubbleSeekBar.getContext(), "end    "+progress, Toast.LENGTH_LONG).show();

               }

               @Override
               public void getProgressOnFinally(BubbleSeekBar bubbleSeekBar, int progress, float progressFloat) {
                   //Toast.makeText(bubbleSeekBar.getContext(), "final"+progress, Toast.LENGTH_LONG).show();
                   setRadios(progress * 1000);
               }
           });
           spinners();
       }
       catch(Exception e){

       }
    }

/*
    public int getChangeSpinnerPhoto() {
        return changeSpinnerPhoto;
    }
*/

    public void setChangeSpinnerPhoto(int changeSpinnerPhoto) {
        this.changeSpinnerPhoto = changeSpinnerPhoto;
    }

    int changeSpinnerPhoto;

    private void spinners() {
        initSpinners();

        mSpinnerAroundMe.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                mSpinnerAroundMe.setBackgroundColor(Color.parseColor("#ECEDF2"));
                getRandomImageInMAin(AROUND_ME_COVER_PHOTO);
                mSpinnerAroundMe.setSelectedIndex(0);


            }
        });
        mSpinnerAroundMe.setOnNothingSelectedListener(new MaterialSpinner.OnNothingSelectedListener() {
            @Override
            public void onNothingSelected(MaterialSpinner spinner) {
                spinner.setBackgroundColor(Color.parseColor("#00000000"));
                getRandomImageInMAin(LAST_COVER_PHOTO);

            }
        });
        mSpinnerAroundMe.setOnItemSelectedListener(new MaterialSpinner.OnItemSelectedListener<String>() {

            @Override
            public void onItemSelected(MaterialSpinner view, int position, long id, String item) {
                if (item.equals(getString(R.string.around_me))){
                    item = "";
                    navigationView.setCheckedItem(R.id.around_me_drawer);

                }
                if (item.equals(getString(R.string.gas_station))){
                    item = "תחנת דלק";
                    navigationView.setCheckedItem(R.id.gas_station);

                }
                if (item.equals(getString(R.string.super_market)))
                    navigationView.setCheckedItem(R.id.supermarket_drawer);
                if (item.equals(getString(R.string.bakery)))
                    navigationView.setCheckedItem(R.id.bakery_drawer);
                if (item.equals(getString(R.string.resturant)))
                    navigationView.setCheckedItem(R.id.resturant_drawer);
                if (item.equals(getString(R.string.park)))
                    navigationView.setCheckedItem(R.id.park_drawer);
                if (item.equals(getString(R.string.coffee_house)))
                    navigationView.setCheckedItem(R.id.cofe_shop_drawer);

                System.out.println("zzzzzzzzzz    "+navigationView.getMenu().getItem(0).getItemId());
                setStr(item);
                designSpinnerRunTime(0);
                setChangeSpinnerPhoto(1);

                if(position==0)
                LAST_COVER_PHOTO = AROUND_ME_COVER_PHOTO;
                if(position==1)
                LAST_COVER_PHOTO = BAKERY_COVER_PHOTO;
                if(position==2)
                LAST_COVER_PHOTO = RESTURANT_COVER_PHOTO;
                if(position==3)
                LAST_COVER_PHOTO = COFE_SHOP_COVER_PHOTO;
               if(position==4)
                LAST_COVER_PHOTO = SUPERMARKET_COVER_PHOTO;
               if(position==5)
                LAST_COVER_PHOTO = PARK_COVER_PHOTO;
                if (position == 6)
                    LAST_COVER_PHOTO = GAS_STATION_COVER_PHOTO;
               getRandomImageInMAin(LAST_COVER_PHOTO);

                // Toast.makeText(context,getStr(),Toast.LENGTH_SHORT).show();
                PlacesDataSource.getPlaces(context, MainActivity.this, location, getStr(), getHours(),getRadios());


            }

        });

        mSpinnerResturant.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                mSpinnerResturant.setBackgroundColor(Color.parseColor("#ECEDF2"));
                getRandomImageInMAin(RESTURANT_COVER_PHOTO);
                mSpinnerResturant.setSelectedIndex(0);

               /* Random rand=new Random();
                int num = rand.nextInt(5 - 0 + 1) + 0;
                if (num == 0)
                    mImageMain.setImageDrawable(getResources().getDrawable(R.drawable.rs3));
                if (num == 1)
                    mImageMain.setImageDrawable(getResources().getDrawable(R.drawable.rs4));
                if (num == 2)
                    mImageMain.setImageDrawable(getResources().getDrawable(R.drawable.rs6));
                if (num == 3)
                    mImageMain.setImageDrawable(getResources().getDrawable(R.drawable.rs7));
                if (num == 4)
                    mImageMain.setImageDrawable(getResources().getDrawable(R.drawable.rs8));

         */
            }
        });
        mSpinnerResturant.setOnNothingSelectedListener(new MaterialSpinner.OnNothingSelectedListener() {
            @Override
            public void onNothingSelected(MaterialSpinner spinner) {
                mSpinnerResturant.setBackgroundColor(Color.parseColor("#00000000"));
                getRandomImageInMAin(LAST_COVER_PHOTO);


            }
        });
        mSpinnerResturant.setOnItemSelectedListener(new MaterialSpinner.OnItemSelectedListener<String>() {

            @Override
            public void onItemSelected(MaterialSpinner view, int position, long id, String item) {
                if (item.equals(getString(R.string.around_me))) {
                    item = getString(R.string.resturant);
                    mSpinnerResturant.setSelectedIndex(0);
                }
                navigationView.setCheckedItem(R.id.resturant_drawer);
                setStr(item);
                designSpinnerRunTime(1);

                setChangeSpinnerPhoto(2);
                LAST_COVER_PHOTO = RESTURANT_COVER_PHOTO;
                PlacesDataSource.getPlaces(context, MainActivity.this, location, getStr(), getHours(),getRadios());

            }
        });

        mSpinnerSuperMarket.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                mSpinnerSuperMarket.setBackgroundColor(Color.parseColor("#ECEDF2"));
               // if(!getResources().getDrawable(R.drawable.sm3).equals(null))
                mImageMain.setImageDrawable(getResources().getDrawable(R.drawable.sm3));
               // mImageMain.setBackgroundResource(R.drawable.sm3);
                //mImageMain.setImageDrawable(getResources().getDrawable(R.drawable.hc1));

                mSpinnerSuperMarket.setSelectedIndex(0);


            }
        });
        mSpinnerSuperMarket.setOnNothingSelectedListener(new MaterialSpinner.OnNothingSelectedListener() {
            @Override
            public void onNothingSelected(MaterialSpinner spinner) {
                mSpinnerSuperMarket.setBackgroundColor(Color.parseColor("#00000000"));
                getRandomImageInMAin(LAST_COVER_PHOTO);


            }
        });
        mSpinnerSuperMarket.setOnItemSelectedListener(new MaterialSpinner.OnItemSelectedListener<String>() {

            @Override
            public void onItemSelected(MaterialSpinner view, int position, long id, String item) {
                if (item.equals(getString(R.string.around_me))) {
                    item = getString(R.string.super_market);
                    mSpinnerSuperMarket.setSelectedIndex(0);
                }
                navigationView.setCheckedItem(R.id.supermarket_drawer);

                setStr(item);
                designSpinnerRunTime(2);

                setChangeSpinnerPhoto(4);
                LAST_COVER_PHOTO = SUPERMARKET_COVER_PHOTO;
                PlacesDataSource.getPlaces(context, MainActivity.this, location, getStr(), getHours(),getRadios());


            }
        });
        mSpinnerBakery.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                mSpinnerBakery.setBackgroundColor(Color.parseColor("#ECEDF2"));
                mSpinnerBakery.setSelectedIndex(0);
/* Random rand = new Random();

                int num = rand.nextInt(3 - 0 + 1) + 0;
                if (num == 0)
                    mImageMain.setImageDrawable(getResources().getDrawable(R.drawable.hc1));
                if (num == 1)
                    mImageMain.setImageDrawable(getResources().getDrawable(R.drawable.hc2));
                if (num == 2)
                    mImageMain.setImageDrawable(getResources().getDrawable(R.drawable.hc3));
*/
                getRandomImageInMAin(BAKERY_COVER_PHOTO);

            }
        });
        mSpinnerBakery.setOnNothingSelectedListener(new MaterialSpinner.OnNothingSelectedListener() {
            @Override
            public void onNothingSelected(MaterialSpinner spinner) {
                mSpinnerBakery.setBackgroundColor(Color.parseColor("#00000000"));
                getRandomImageInMAin(LAST_COVER_PHOTO);


            }
        });
        mSpinnerBakery.setOnItemSelectedListener(new MaterialSpinner.OnItemSelectedListener<String>() {

            @Override
            public void onItemSelected(MaterialSpinner view, int position, long id, String item) {
                if (item.equals(getString(R.string.around_me))) {
                    item = getString(R.string.coffee_house);
                    mSpinnerBakery.setSelectedIndex(0);
                }
                navigationView.setCheckedItem(R.id.cofe_shop_drawer);

                setStr(item);
                designSpinnerRunTime(3);

                setChangeSpinnerPhoto(3);
                LAST_COVER_PHOTO = BAKERY_COVER_PHOTO;
                PlacesDataSource.getPlaces(context, MainActivity.this, location, getStr(), getHours(),getRadios());

            }
        });

    }

    private void initSpinners() {
        ///the linear is a background of spinners
        mImageMain = findViewById(R.id.image_top_spinners);

        mLinear_around_me = findViewById(R.id.linear_around_me);
        mLinear_resturant = findViewById(R.id.linear_resturant);
        mLinear_supermarket = findViewById(R.id.linear_supermarket);
        mLinear_bakery = findViewById(R.id.linear_bakery);
        mSpinnerAroundMe = (MaterialSpinner) findViewById(R.id.spinner_around_me);
        mSpinnerAroundMe.setItems(getString(R.string.around_me), getString(R.string.bakery), getString(R.string.resturant), getString(R.string.coffee_house), getString(R.string.super_market), getString(R.string.park),getString(R.string.gas_station));
        mSpinnerBakery = (MaterialSpinner) findViewById(R.id.spinner_bakery);
        mSpinnerBakery.setItems(getString(R.string.coffee_house), getString(R.string.around_me), "ארומה אספרסו בר", "קפה ג'ו", "קפה לנדוור", "קפה קפה");

        mSpinnerSuperMarket = (MaterialSpinner) findViewById(R.id.spinner_supermarket);
        mSpinnerSuperMarket.setItems(getString(R.string.super_market), getString(R.string.around_me), "אלמשהדאוי", "שופרסל", "רמי לוי", "יינות ביתן");
        mSpinnerResturant = (MaterialSpinner) findViewById(R.id.spinner_resturant);
        mSpinnerResturant.setItems(getString(R.string.resturant), getString(R.string.around_me), "BBB", "mcdonalds", "eight");

    }

    ArrayList mSpinnerArray;
    ArrayList mLinearOfSpinnersArray;

    private void designSpinnerRunTime(int numSpinner) {
        //numSpinner that > sizeof array that says it found just in drawer!! not found in specific spinner
        mSpinnerArray = new ArrayList<MaterialSpinner>();
        mLinearOfSpinnersArray = new ArrayList<LinearLayout>();
        mSpinnerArray.add(mSpinnerAroundMe);
        mSpinnerArray.add(mSpinnerResturant);
        mSpinnerArray.add(mSpinnerSuperMarket);
        mSpinnerArray.add(mSpinnerBakery);
        mLinearOfSpinnersArray.add(mLinear_around_me);
        mLinearOfSpinnersArray.add(mLinear_resturant);
        mLinearOfSpinnersArray.add(mLinear_supermarket);
        mLinearOfSpinnersArray.add(mLinear_bakery);
        if (numSpinner < mSpinnerArray.size()) {
            int j = 0;
            for (int i = 0; i < mSpinnerArray.size(); i++) {
                if (i == numSpinner) {

                    ((MaterialSpinner) mSpinnerArray.get(i)).setBackgroundColor(Color.parseColor("#00000000"));

                    i++;
                }
                j = i;
                if (i == mSpinnerArray.size())//if lastofindex
                    j = mSpinnerArray.size() - 2;
                ((MaterialSpinner) mSpinnerArray.get(j)).setSelectedIndex(0);

            }
        } else {
            for (int i = 0; i < mLinearOfSpinnersArray.size(); i++)
                //if swip from drawr
                ((MaterialSpinner) mSpinnerArray.get(i)).setSelectedIndex(0);

        }
        for (int i = 0; i < mLinearOfSpinnersArray.size(); i++) {
            if (i == numSpinner)
                ((LinearLayout) mLinearOfSpinnersArray.get(i)).setBackground(getResources().getDrawable(R.drawable.rectangle_affor_white_borders_black));
            else
                ((LinearLayout) mLinearOfSpinnersArray.get(i)).setBackground(getResources().getDrawable(R.drawable.rectangle_white_borders_blue));

        }
    }

    private void mSearch() {
        mSearch = findViewById(R.id.li2);
        mSearch.setInputType(0);
        mSearch.setOnTouchListener(new View.OnTouchListener() {
            @Override
            public boolean onTouch(View v, MotionEvent event) {
                mSearch.setInputType(1);

                return false;
            }
        });
        mSearch.setOnFocusChangeListener(new View.OnFocusChangeListener() {

            @Override
            public void onFocusChange(View v, boolean hasFocus) {
                /* When focus is lost check that the text field
                 * has valid values.
                 */
                if (mSearch.getText().length() != 0)
                    mClose.setVisibility(View.VISIBLE);

                if (!hasFocus) {


                    mClose.setVisibility(View.INVISIBLE);

                    setStr(mSearch.getText().toString());

                    // Toast.makeText(getBaseContext(),"1111"+getStr(),Toast.LENGTH_LONG).show();
                    mSearch.setImeOptions(EditorInfo.IME_ACTION_DONE);

                    if (mSearch.getImeOptions() == IME_ACTION_DONE) {
                        ((InputMethodManager) MainActivity.this.getSystemService(Context.INPUT_METHOD_SERVICE)).hideSoftInputFromWindow(v.getWindowToken(), 0);
                        ob.setCol(0);
                        PlacesDataSource.getPlaces(context, MainActivity.this, location, getStr(), getHours(),getRadios());

                    }


                } else {
                    //if(mSearch.getText().length()>0)
                    mClose.setVisibility(View.VISIBLE);
                  /*  else
                        mClose.setVisibility(View.VISIBLE);
*/
                    mSearch.setImeOptions(EditorInfo.IME_ACTION_SEARCH);

                    setStr(mSearch.getText().toString());

                    //Toast.makeText(getBaseContext(),"2222"+getStr(),Toast.LENGTH_LONG).show();
                   /* str=  mSearch.getText().toString();
                    Toast.makeText(getBaseContext(),str,Toast.LENGTH_LONG).show();*/
                }

            }
        });
        mSearch.addTextChangedListener(new TextWatcher() {
            @Override
            public void beforeTextChanged(CharSequence s, int start, int count, int after) {

            }

            @Override
            public void onTextChanged(CharSequence s, int start, int before, int count) {
                if (s.length() != 0) {
                    mClose.setVisibility(View.VISIBLE);
                } else {
                    mClose.setVisibility(View.INVISIBLE);
                }
            }

            @Override
            public void afterTextChanged(Editable s) {

            }
        });

    }

    private void topListPlaces() {
        mTopList = findViewById(R.id.topp);
        mTopList.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                setSelected(5);
                if(rvPalaces!=null)
                rvPalaces.getLayoutManager().scrollToPosition(0);
                /* for go to top of the list */
            }
        });

    }

    private void mSwipeContainer() {
        //this func for the referesh by slide bottum
        mSwipeContainer = findViewById(R.id.swipe_container);
        mSwipeContainer.setOnRefreshListener(
                new SwipeRefreshLayout.OnRefreshListener() {
                    @Override
                    public void onRefresh() {
                        doYourUpdate();
                    }
                }
                //to do update for the recycle
        );
    }

    private void mClose() {
        mClose = findViewById(R.id.close);
        mClose.setOnClickListener(new View.OnClickListener() {

            @Override
            public void onClick(View view) {
                mSearch.setText("");
                mSearch.getText().clear();
                //mSearch.setInputType(InputType.TYPE_NULL);

                mClose.setVisibility(View.INVISIBLE);

            }
        });

    }

    private void mCategory() {
        mCategory = findViewById(R.id.btn_drawer);
        mCategory.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View view) {

                mDrawer.openMenu(true, 2);

                // Toast.makeText(context, "kbeeeeel " + getStr(), Toast.LENGTH_LONG).show();
                //  setSelected(3);
                //   drawer.openDrawer(Gravity.RIGHT); //Edit Gravity.START need API 14


                //PopUpDebriefingSideViewDialog my = new PopUpDebriefingSideViewDialog(context, MainActivity.this, location, "xxx", getHours());

//            rvPalaces.getLayoutManager().scrollToPosition(0);

            }

        });

    }

    private void mBtnFile() {
        mBtnFile = findViewById(R.id.ii);
        mBtnFile.setOnClickListener(v -> {

            setHours("&opennow=true");
            PlacesDataSource.getPlaces(context, MainActivity.this, location, getStr(), getHours(),getRadios());

            setSelected(2);

        });
    }

    private void mBtnHome() {
        mBtnHome = findViewById(R.id.textView);

        mBtnHome.setOnClickListener(v -> {

            setHours("");
            setRadios(0);
            mBbubbleSeekBar.setProgress(0);
            // Toast.makeText(MainActivity.this, getStr(), Toast.LENGTH_SHORT).show();

            PlacesDataSource.getPlaces(context, MainActivity.this, location, getStr(), getHours(),getRadios());

            setSelected(1);


        });
    }


    @Override
    protected void onResume() {
        super.onResume();
        if (dialog != null)
            dialog.dismiss();

        if (this.mGoogleApiClient != null) {
            mGoogleApiClient.connect();
        }

        if (ob.getCount() != 0) {
            b = 10;
            // PlacesDataSource.getPlaces(context, MainActivity.this, location, getStr(), getHours());

        }

    }

    @Override
    protected void onStop() {
        // Disconnecting the client invalidates it.
        if (mGoogleApiClient != null) {
            mGoogleApiClient.disconnect();
        }
        super.onStop();
    }

    @Override
    public void onPause() {
        super.onPause();
        //stop location updates when Activity is no longer active
        if (mGoogleApiClient != null) {
            LocationServices.FusedLocationApi.removeLocationUpdates(mGoogleApiClient, this);
        }
    }

    public synchronized void buildGoogleApiClient() {
        int checkPermission = ActivityCompat.checkSelfPermission(
                this, Manifest.permission.ACCESS_FINE_LOCATION); //BitMask
        //if no permission -> Reuqest the permission (And Return from the method)
        if (checkPermission != PackageManager.PERMISSION_GRANTED) {
            //Request the permission:
            ActivityCompat.requestPermissions(
                    this, new String[]{Manifest.permission.ACCESS_FINE_LOCATION}, RC_REQUEST_LOCATION);
            return;//get out of the method
        }
        mGoogleApiClient = new GoogleApiClient.Builder(getBaseContext())
                .addConnectionCallbacks(this)
                .addOnConnectionFailedListener(this)
                .addApi(LocationServices.API)
                .build();
        mGoogleApiClient.connect();
    }


    @SuppressLint("MissingPermission")
    @Override
    public void onConnected(Bundle bundle) {
        //  try {
        mLocationRequest = LocationRequest.create();
        mLocationRequest.setPriority(LocationRequest.PRIORITY_HIGH_ACCURACY);
        mLocationRequest.setInterval(10); // Update location every second


        FusedLocationApi.requestLocationUpdates(mGoogleApiClient, mLocationRequest, this);
        //  Toast.makeText(context,"gps asdasdasdsadas",Toast.LENGTH_LONG).show();
        dialog = new Dialog(context);

        dialog.setContentView(R.layout.dialog_item);
        dialog.setTitle("Custom Alert Dialog");
        if (FusedLocationApi.getLastLocation(mGoogleApiClient) == null) {
          /*  builder.setMessage("للحصول على المعلومات قم بتفعيل خدمة الاماكن").setIcon(R.drawable.x).
                    setTitle("خدمة الاماكن لا تعمل").setPositiveButton("نعم", new DialogInterface.OnClickListener() {
                @Override
                public void onClick(DialogInterface dialog, int which) {
                    Toast.makeText(context, "yees", Toast.LENGTH_LONG).show();
                    Intent callGPSSettingIntent = new Intent(
                            android.provider.Settings.ACTION_LOCATION_SOURCE_SETTINGS);
                    startActivity(callGPSSettingIntent);
                }
            }).setNegativeButton("لا", new DialogInterface.OnClickListener() {
                @Override
                public void onClick(DialogInterface dialog, int which) {
                    Toast.makeText(context,"nooooooo",Toast.LENGTH_LONG).show();

                }
            }).show();*/
            //dialog.show();

            showGPSDisabledAlertToUser();

        } else {
            location = new Location(FusedLocationApi.getLastLocation(mGoogleApiClient));
            dialog.cancel();

            dialog.dismiss();
        }//  }


        if (FusedLocationApi.getLastLocation(mGoogleApiClient) != null) {
            dialog.dismiss();
        }


    }

    private void showGPSDisabledAlertToUser() {
        AlertDialog.Builder alertDialogBuilder = new AlertDialog.Builder(this);
        alertDialogBuilder.setMessage(getResources().getString(R.string.location_not_work))
                .setCancelable(true)
                .setPositiveButton(getResources().getString(R.string.ask_gps),
                        new DialogInterface.OnClickListener() {
                            public void onClick(DialogInterface dialog, int id) {
                                Intent callGPSSettingIntent = new Intent(
                                        android.provider.Settings.ACTION_LOCATION_SOURCE_SETTINGS);
                                startActivity(callGPSSettingIntent);

                            }
                        });
        alertDialogBuilder.setNegativeButton(getResources().getString(R.string.cancel_gps),
                new DialogInterface.OnClickListener() {
                    public void onClick(DialogInterface dialog, int id) {
                        dialog.cancel();
                    }
                });
        AlertDialog alert = alertDialogBuilder.create();
        alert.show();
    }

    @Override
    public void onConnectionSuspended(int i) {
        //Log.i(LOG_TAG, "GoogleApiClient connection has been suspend");
    }

    @Override
    public void onConnectionFailed(ConnectionResult connectionResult) {
        //Log.i(LOG_TAG, "GoogleApiClient connection has failed");
    }


    @Override
    public void onLocationChanged(Location location) {
        this.location = new Location(location);
        //Log.i(LOG_TAG, this.location.toString());

        if (b == 0) {
            // whene start the app get places and after that dis able the video
            //PlacesDataSource.getPlaces(context, MainActivity.this, location, getStr(), getHours());
            ob.setCount(1);

            PlacesDataSource.getPlaces(context, MainActivity.this, location, getStr(), getHours(),getRadios());
            // mVideoview.setVisibility(View.INVISIBLE);
            // mBtnClos.setVisibility(View.INVISIBLE);
            dialog.dismiss();

        }
       /* if (b == 10) {
            // whene resume the app get places
            final Dialog dialog22 = new Dialog(context);

            dialog22.setContentView(R.layout.dialog_refresh);
            dialog22.setTitle("Custom Alert Dialog");
            final ImageView reef = dialog22.findViewById(R.id.im_ref);
            reef.setOnClickListener(new View.OnClickListener() {
                @Override
                public void onClick(View v) {
                    PlacesDataSource.getPlaces(context, MainActivity.this, location, getStr(), getHours());
                    dialog22.dismiss();
                }
            });


            dialog22.show();


        }*/
        System.out.println("rrrrrr   " + location.getLatitude());


        b = 2;


        //txtOutput.setText(location.toString());

    }

    private void requestLocation() {
        int checkPermission = ActivityCompat.checkSelfPermission(
                this, Manifest.permission.ACCESS_FINE_LOCATION); //BitMask
        //if no permission -> Reuqest the permission (And Return from the method)
        if (checkPermission != PackageManager.PERMISSION_GRANTED) {
            //Request the permission:
            ActivityCompat.requestPermissions(
                    this, new String[]{Manifest.permission.ACCESS_FINE_LOCATION}, RC_REQUEST_LOCATION);
            return;//get out of the method
        }
        requestLocationUpdates();
    }

    private void requestLocationUpdates() {
        mLocationRequest = LocationRequest.create();
        mLocationRequest.setPriority(LocationRequest.PRIORITY_HIGH_ACCURACY);
        mLocationRequest.setInterval(10); // Update location every second

        if (ActivityCompat.checkSelfPermission(
                this, Manifest.permission.ACCESS_FINE_LOCATION) !=
                PackageManager.PERMISSION_GRANTED && ActivityCompat.checkSelfPermission(
                this, android.Manifest.permission.ACCESS_COARSE_LOCATION) !=
                PackageManager.PERMISSION_GRANTED) {
            return;
        }

        FusedLocationApi.requestLocationUpdates(mGoogleApiClient, mLocationRequest, this);
        location = new Location(FusedLocationApi.getLastLocation(mGoogleApiClient));
        //PlacesDataSource.getPlaces(context, MainActivity.this, location, getStr(), getHours());

    }


    @Override
    public boolean onNavigationItemSelected(MenuItem item) {
        // Handle navigation view item clicks here.
        //numSpinner that > sizeof array that says it found just in drawer!! not found in specific spinner

        int id = item.getItemId();
        switch (id) {
            case R.id.around_me_drawer:
                setStr("");
                LAST_COVER_PHOTO = AROUND_ME_COVER_PHOTO;
                PlacesDataSource.getPlaces(context, MainActivity.this, location, getStr(), getHours(),getRadios());
                designSpinnerRunTime(0);
                getRandomImageInMAin(AROUND_ME_COVER_PHOTO);
                break;
            case R.id.resturant_drawer:
                designSpinnerRunTime(1);

                setStr(getResources().getString(R.string.resturant));
                LAST_COVER_PHOTO = RESTURANT_COVER_PHOTO;
                PlacesDataSource.getPlaces(context, MainActivity.this, location, getStr(), getHours(),getRadios());
                getRandomImageInMAin(RESTURANT_COVER_PHOTO);
                break;
            case R.id.bakery_drawer:
                setStr(getResources().getString(R.string.bakery));
                LAST_COVER_PHOTO = BAKERY_COVER_PHOTO;
                PlacesDataSource.getPlaces(context, MainActivity.this, location, getStr(), getHours(),getRadios());
                if (mSpinnerArray != null)
                    designSpinnerRunTime(mSpinnerArray.size() + 2);//becuase not found in spinner
                getRandomImageInMAin(BAKERY_COVER_PHOTO);
                break;
            case R.id.supermarket_drawer:
                setStr(getResources().getString(R.string.super_market));
                designSpinnerRunTime(2);
                LAST_COVER_PHOTO = SUPERMARKET_COVER_PHOTO;
                getRandomImageInMAin(SUPERMARKET_COVER_PHOTO);
                PlacesDataSource.getPlaces(context, MainActivity.this, location, getStr(), getHours(),getRadios());
                break;
            case R.id.cofe_shop_drawer:

                setStr(getResources().getString(R.string.coffee_house));
                designSpinnerRunTime(3);
                LAST_COVER_PHOTO = COFE_SHOP_COVER_PHOTO;
                getRandomImageInMAin(COFE_SHOP_COVER_PHOTO);
                PlacesDataSource.getPlaces(context, MainActivity.this, location, getStr(), getHours(),getRadios());

                break;
            case R.id.park_drawer:
                setStr(getResources().getString(R.string.park));
                if (mSpinnerArray != null)
                    designSpinnerRunTime(mSpinnerArray.size() + 3);
                LAST_COVER_PHOTO = PARK_COVER_PHOTO;
                getRandomImageInMAin(PARK_COVER_PHOTO);
                PlacesDataSource.getPlaces(context, MainActivity.this, location, getStr(), getHours(),getRadios());
                break;
            case R.id.gas_station:
                setStr("תחנת דלק");
                if (mSpinnerArray != null)
                    designSpinnerRunTime(mSpinnerArray.size() + 4);
                LAST_COVER_PHOTO = GAS_STATION_COVER_PHOTO;
                getRandomImageInMAin(GAS_STATION_COVER_PHOTO);
                PlacesDataSource.getPlaces(context, MainActivity.this, location, getStr(), getHours(), getRadios());
                break;

            //the upper part is used for fragment transactions.
//                Two parts to the nav, the lower is used for intents.
            case R.id.nav_share:
                //TODO: Add sharing" Settings Activity...";

                Intent i = new Intent(Intent.ACTION_SEND);
                i.setType("text/plain");
                i.putExtra(Intent.EXTRA_SUBJECT, "Nasamat");
                String message = "\n" + getResources().getString(R.string.share_msg) + "\n https://play.google.com/store/apps/details?id=com.amr.nearbyopen \n\n";

                i.putExtra(Intent.EXTRA_TEXT, message);
                startActivity(Intent.createChooser(i, getResources().getString(R.string.choose_one)));
                break;

            case R.id.nav_share2:
                Intent intent = new Intent(Intent.ACTION_SENDTO, Uri.fromParts(
                        "mailto", "nasamat.w@gmail.com", null));
                intent.putExtra(Intent.EXTRA_SUBJECT, "");
                intent.putExtra(Intent.EXTRA_TEXT, "");
                startActivity(Intent.createChooser(intent, getResources().getString(R.string.choose_one)));
        }
        //mDrawer = (FlowingDrawer) findViewById(R.id.drawerlayout);

        // FlowingDrawer   mdrawer = (FlowingDrawer)findViewById(R.id.drawer_layout);
        mDrawer.closeMenu(true, 1);//.closeDrawer(GravityCompat.END);
        return true;
    }
}
