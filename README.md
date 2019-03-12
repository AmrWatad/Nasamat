#Nasamat

#this is the Main of my App,Nasamat Android App developed in Java on Android Studio that displays nearby places with details(distance,rating,is open,phone,photos)using threads,location,Rest API with Json,design patterns and other libraries for UI/UX:Picasso,Flowing-Drawer,Swipy

#you can find it on google paly store:#https://play.google.com/store/apps/details?id=com.amr.nearbyopen



import com.mxn.soul.flowingdrawer_core.FlowingDrawer;*
import com.jaredrummler.materialspinner.MaterialSpinner;*
import com.google.android.gms.common.api.GoogleApiClient;*
import io.fabric.sdk.android.Fabric;*
import static com.google.android.gms.location.LocationServices.*;*
import com.mxn.soul.flowingdrawer_core.FlowingDrawer;*
import com.xw.repo.BubbleSeekBar;



public class MainActivity extends Activity implements PlacesDataSource.OnPlacesArrivedListener,
		GoogleApiClient.ConnectionCallbacks,#GoogleApiClient.OnConnectionFailedListener,LocationListener,NavigationView.OnNavigationItemSelectedListener
{

	@Override
	public void onCreate(Bundle savedInstanceState) {
		super.onCreate(savedInstanceState);
		Fabric.with(this, new Crashlytics());
		toastMessageWel();

		try {
			buildGoogleApiClient();
			setContentView(R.layout.activity_main);
			radiosKm();
			drawerFunc();
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

	public void onPlacesArrived(@Nullable final ArrayList<Place> places, @Nullable final Exception e) {
		// The places are received in a background thread.

		// runOnUIThread(Runnable runnable)
		// run on UI thread... a method that runs code on the ui (main) thread.

		runOnUiThread(new Runnable() {

			@Override
			public void run() {

				// code that runs on the UI (Main) Thread...!

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
	public void onConfigurationChanged(Configuration newConfig) {
		super.onConfigurationChanged(newConfig);

		// when open app the keypoared display up this func hide the keypored
	}

	@Override
	public void onRequestPermissionsResult(int requestCode, @NonNull String[] permissions,
			@NonNull int[] grantResults) {
		super.onRequestPermissionsResult(requestCode, permissions, grantResults);
		if (requestCode == RC_REQUEST_LOCATION && grantResults[0] == PackageManager.PERMISSION_GRANTED) {
			buildGoogleApiClient();
		}
		if (requestCode == MY_PERMISSIONS_REQUEST_READ_CONTACTS) {
			if (grantResults[0] == PackageManager.PERMISSION_GRANTED) {
				phoneCall();
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
		}
	}

	public Location getLocation() {
		return MainActivity.this.location;
	}

	private void doYourUpdate() { // this for the referesh when slide down
		// TODO implement a refresh
		PlacesDataSource.getPlaces(context, MainActivity.this, location, getStr(), getHours(), getRadios());
		mSwipeContainer.setRefreshing(false); // Disables the refresh icon

	}

	private void drawerFunc() {
		navigationView = findViewById(R.id.nav_view);
		navigationView.setNavigationItemSelectedListener(this);
		navigationView.setCheckedItem(R.id.around_me_drawer);
		DrawerLayout drawer = findViewById(R.id.drawer_layout);
		mDrawer = (FlowingDrawer) findViewById(R.id.drawerlayout);
		mDrawer.setTouchMode(ElasticDrawer.TOUCH_MODE_BEZEL);
		// https://github.com/mxn21/FlowingDrawer
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
		if (mGoogleApiClient != null) {
			LocationServices.FusedLocationApi.removeLocationUpdates(mGoogleApiClient, this);
		}
	}

	public synchronized void buildGoogleApiClient() {
		int checkPermission = ActivityCompat.checkSelfPermission(this, Manifest.permission.ACCESS_FINE_LOCATION); // BitMask
		if (checkPermission != PackageManager.PERMISSION_GRANTED) {
			ActivityCompat.requestPermissions(this, new String[] { Manifest.permission.ACCESS_FINE_LOCATION },
					RC_REQUEST_LOCATION);
			return;// get out of the method
		}
		mGoogleApiClient = new GoogleApiClient.Builder(getBaseContext()).addConnectionCallbacks(this)
				.addOnConnectionFailedListener(this).addApi(LocationServices.API).build();
		mGoogleApiClient.connect();
	}

	@SuppressLint("MissingPermission")
	@Override
	public void onConnected(Bundle bundle) {
		// try {
		mLocationRequest = LocationRequest.create();
		mLocationRequest.setPriority(LocationRequest.PRIORITY_HIGH_ACCURACY);
		mLocationRequest.setInterval(10);

		FusedLocationApi.requestLocationUpdates(mGoogleApiClient, mLocationRequest, this);
		dialog = new Dialog(context);

		dialog.setContentView(R.layout.dialog_item);
		dialog.setTitle("Custom Alert Dialog");
		if (FusedLocationApi.getLastLocation(mGoogleApiClient) == null) {

			showGPSDisabledAlertToUser();

		} else {
			location = new Location(FusedLocationApi.getLastLocation(mGoogleApiClient));
			dialog.cancel();

			dialog.dismiss();
		}

		if (FusedLocationApi.getLastLocation(mGoogleApiClient) != null) {
			dialog.dismiss();
		}

	}

	private void showGPSDisabledAlertToUser() {
		AlertDialog.Builder alertDialogBuilder = new AlertDialog.Builder(this);
		alertDialogBuilder.setMessage(getResources().getString(R.string.location_not_work)).setCancelable(true)
				.setPositiveButton(getResources().getString(R.string.ask_gps), new DialogInterface.OnClickListener() {
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
	}

	@Override
	public void onConnectionFailed(ConnectionResult connectionResult) {
		// Log.i(LOG_TAG, "GoogleApiClient connection has failed");
	}

	private void requestLocationUpdates() {
		mLocationRequest = LocationRequest.create();
		mLocationRequest.setPriority(LocationRequest.PRIORITY_HIGH_ACCURACY);
		mLocationRequest.setInterval(10); // Update location every second

		if (ActivityCompat.checkSelfPermission(this,
				Manifest.permission.ACCESS_FINE_LOCATION) != PackageManager.PERMISSION_GRANTED
				&& ActivityCompat.checkSelfPermission(this,
						android.Manifest.permission.ACCESS_COARSE_LOCATION) != PackageManager.PERMISSION_GRANTED) {
			return;
		}

		FusedLocationApi.requestLocationUpdates(mGoogleApiClient, mLocationRequest, this);
		location = new Location(FusedLocationApi.getLastLocation(mGoogleApiClient));

	}

}
