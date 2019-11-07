# rtCamp Project
**Complex PSD to WordPress Assignment**


Website link - http://davincidesigns.site/rtcamp/


This is the complex PSD to WordPress hiring assignment required by rtCamp in order to apply for the WP Developer position.

## 3rd Party Resources

1. **Owl Carousel**

     - This is a jquery carousel plugin which is used for many sliders in the homepage.

     - *Plugin Link* - https://owlcarousel2.github.io/OwlCarousel2/

2. **FontAwesome Icon Library**

   - This is a library of vector icons.

   - *Resource Link* - https://fontawesome.com/

3. **Bootstrap Grid System**

   - As required by the assignment, the grid system of the bootstrap is used throughout the website.

   - *Resource Link* - https://getbootstrap.com/

4. **Thickbox**

   - As required by the assignment, thickbox is used for the YouTube video sliders

   - *Plugin Link* - http://codylindley.com/thickbox/

5. **Blank theme**

   - As required by the assignment, a blank template from Underscores.me

   - *Template Link* - http://underscores.me/
   
   
6. **ACF**

   - ACF was used to create an option page with a set of custom fields

   - *Plugin Link* - https://www.advancedcustomfields.com/


7. **CPT UI**

   - CPT UI was used to create the required custom post types

   - *Plugin Link* - https://wordpress.org/plugins/custom-post-type-ui/
   

8. **OpenWeatherMap API**

   - OpenWeatherMap API was used to fetch weather data of a specified city.

   - *API Link* - https://openweathermap.org/
   

7. **Google Geocoding API & TimeZone API**

   - Geocoding & timezone api were used in a widget to get the current date & time of a city specified in the backend.

   - *API Link* - https://developers.google.com/maps/documentation/timezone/start
   - *API Link* - https://developers.google.com/maps/documentation/geocoding/start
   
   
   
## Getting Started
   
   Initially all the resource files required by the project scope were loaded into the template. Right after the HTML markup was made for all the required sections, columns and content. Bootstrap grid played an important role here. 
   
   Once the markup was complete, using CSS, the website was styled to identically match the PSD provided. 
  
  
  
## HomePage Slider

A custom field (Text Area) was create on the Theme options page where the user is supposed to enter each youtube video id in a new line. Once the settings are saved, the string is split using new lines to get each video ID into an array which will be used in the youtube video slider.

```
<?php

echo '<div class="youtube_slider_parent owl-carousel owl-theme">' ;	// Youtube Slider 

$links = get_field('theme_youtube_slider_links' , 'option' ) ;
$links = preg_split("/\r\n|\n|\r/", $links) ;

foreach( $links as $link ): 

	?>

	<a class="youtube_slider_single_item thickbox" href="https://www.youtube.com/embed/<?php echo $link ; ?>?rel=0?KeepThis=true&TB_iframe=true&height=400&width=600">	<!-- youtube single item -->
	
		<img class="youtube_slider_single_item_img" src="<?php echo 'https://img.youtube.com/vi/' . $link . '/hqdefault.jpg' ; ?>">
		<img class="youtube_slider_single_item_icon" src="<?php echo get_stylesheet_directory_uri() . '/lib/play.png' ; ?>">

	</a>	<!-- youtube single item -->

	<?php

endforeach; 

echo '</div>' ;	// Youtube Slider 

?>
```


The classname ***thickbox*** is used as a class selector for the thickbox script. The ***href*** attribute is used for the thickbox script to identify what to display.

```
// Youtube Slider Owl Carousel Init

$('.youtube_slider_parent.owl-carousel').owlCarousel({
    loop:false,
    margin: 10,
    nav: true,
    dots: false,
    autoplay:true,
    autoplayTimeout: 10000,
    autoplayHoverPause:true,
    navText : ["<i class='fa fa-chevron-left'></i>","<i class='fa fa-chevron-right'></i>"],
    responsiveClass:true,
	responsive:{
		0:{
	        items:1
	    },
	    300:{
	        items:2
	    },
	    600:{
	        items:3
	    },
	    800:{
	        items:4
	    },
	    1000:{
	        items:5
	    }
	}
})

```


## Glimpses of Exhibition Section

A custom post type "Exhibitions" was created. The items displayed in this section are pulled from the "Exhibition" posts usin wp_query.

```
<?php 	// Custom Post Type Thumbnails

	$args = array(  
			'post_type' => 'exhibition',
			'post_status' => 'publish',
			'posts_per_page' => 8, 
			'order' => 'DESC',
			'orderby' => 'date', 
			) ;

	$new_query = new WP_Query( $args ); 

	?>

	<h3 class="colored_title">Glimpses of Exhibition</h3>

	<div class="row">	<!-- Row -->

	<?php
	    
	while ( $new_query->have_posts() ) : 

		$new_query->the_post(); 

		?>

		<div class="col-md-3 col-xs-6">
		
			<a class="hp_glimpses_single_box" href="<?php the_permalink() ; ?>">
				<img src="<?php echo get_the_post_thumbnail_url(get_the_ID(),'medium') ; ?>">
			</a>

			<a href="<?php the_permalink() ; ?>" class="hp_glimpses_single_linker"><?php the_title() ; ?></a>

		</div>

		<?php

	endwhile;

	?>

	</div>	<!-- Row -->

	<div class="hruler"></div>

	<?php

	wp_reset_postdata(); 

?>	<!-- Custom Post Type Thumbnails -->
```
   
   
## Latest Tweets

A 3rd party plugin named "Custom Twitter Feeds" was used to display the latest tweets.


## FaceBook Like box

The feed is generated by a script customized at the FaceBook social media plugin page.

```
<div class="fb-page" data-href="https://www.facebook.com/rushmovie" data-tabs="" data-width="" data-height="250" data-small-header="false" data-adapt-container-width="true" data-hide-cover="false" data-show-facepile="true"><blockquote cite="https://www.facebook.com/rushmovie" class="fb-xfbml-parse-ignore"><a href="https://www.facebook.com/rushmovie">RUSH</a></blockquote></div>
```


## Sidebars and Widget Areas

A sidebar and other widget areas were registered so that the widgets could be placed at required positions. 

```
// Widgetized Sidebar Area

function rtcamp_customwidget_area() {
 
    register_sidebar( array(
        'name'          => 'rtCamp Homepage Sidebar',
        'id'            => 'rtcamp-homepage-sidebar',
        'before_widget' => '<div class="rtcamp-widget">',
        'after_widget'  => '</div>',
        'before_title'  => '<h2 class="rtcamp-title">',
        'after_title'   => '</h2>',
    ) );
 
}

add_action( 'widgets_init', 'rtcamp_customwidget_area' );
```


## Custom Widgets

Multiple custom widgets were built for this template as required by the PSD. The widgets were created using the minimum of 4 classes that is required to build a widget. 

```
<?php

class News_posts_widget extends WP_Widget {

	
	public function __construct() {
		$widget_ops = array( 
			'classname' => 'news_posts_widget',
			'description' => 'This widget displays all posts from the "News" category with a dynamic navigation',
		);
		parent::__construct( 'news_posts_widget', 'rtCamp News Posts', $widget_ops );
	}

	
	public function widget( $args, $instance ) {

		echo $args['before_widget'];
		
		if ( ! empty( $instance['title'] ) ) {
			echo $args['before_title'] . apply_filters( 'widget_title', $instance['title'] ) . $args['after_title'];
		}
 
		<?php

		echo $args['after_widget'];
	
	}

	
	public function form( $instance ) {

		$title = ! empty( $instance['title'] ) ? $instance['title'] : esc_html__( 'News', 'text_domain' );

		?>

		<input class="widefat" id="<?php echo esc_attr( $this->get_field_id( 'title' ) ); ?>" name="<?php echo esc_attr( $this->get_field_name( 'title' ) ); ?>" type="text" value="<?php echo esc_attr( $title ); ?>">

		<?php
		
	}

	
	public function update( $new_instance, $old_instance ) {
	
		$instance = array();
		$instance['title'] = ( ! empty( $new_instance['title'] ) ) ? strip_tags( $new_instance['title'] ) : '';

		return $instance;
	}

}

// register News_posts_widget

add_action( 'widgets_init', function(){
	register_widget( 'News_posts_widget' );
});
```


## News posts with dynamic navigation

The Ajax Url was first defined in the functions.php file.

``` 
add_action('wp_head', 'myplugin_ajaxurl');

function myplugin_ajaxurl() {

   echo '<script type="text/javascript">
           var ajaxurl = "' . admin_url('admin-ajax.php') . '";
         </script>';
}
```

Using Jquery and PHP, an ajax function was created to allow navigating through the News posts, 5 posts at a time.


## Weather widget

OpenWeathermap api was used to fetch the weather data for 3 days using the city ID specified in the back end. Since the weather icons seen at the PSD were not provided, custom icons are used to display the appropriate weather.

```
<?php

$cityId = $instance['city_id'] ;

$apiKey = "Your_api_key";

$googleApiUrl = "https://api.openweathermap.org/data/2.5/forecast?id=" . $cityId . "&lang=en&units=metric&APPID=" . $apiKey . "&cnt=24" ;

$ch = curl_init();

curl_setopt($ch, CURLOPT_HEADER, 0);
curl_setopt($ch, CURLOPT_RETURNTRANSFER, 1);
curl_setopt($ch, CURLOPT_URL, $googleApiUrl);
curl_setopt($ch, CURLOPT_FOLLOWLOCATION, 1);
curl_setopt($ch, CURLOPT_VERBOSE, 0);
curl_setopt($ch, CURLOPT_SSL_VERIFYPEER, false);
$response = curl_exec($ch);

curl_close($ch);
$data = json_decode($response);


?>

<span class="weather_single_day_stat"> <?php echo $data->list[5]->weather[0]->description ; ?> </span>
<span class="weather_single_day_temp"> <?php echo $data->list[5]->main->temp ; ?> °C</span>
```


## Date Time widget

The Geocoding api was used to fetch the latitude & longitudinal values of the specified city. 

```
$address = $cityNamer ;
			 
$api_url = "https://maps.googleapis.com/maps/api/geocode/json?address={$address}&key=AIzaSyDBDSDw19iYcE3LEBbmScok9xGFV0RVMrE";

$resp_json = file_get_contents($api_url);

$resp = json_decode($resp_json, true);

if( $resp['status']=='OK' ) {	// If city is valid from Geocoding API response
     
    $lati = $resp['results'][0]['geometry']['location']['lat'] ;
    $longi = $resp['results'][0]['geometry']['location']['lng'] ;

}
```

Once the coordinates were received, they were fed to the timezone api to get the current date & time of the city specified in the backend.


## Footer link shortcode

The shortcode was created using the following code to achieve the desired result.

```
// Footer Custom Link Shortcode


function footer_link_shortcode( $atts, $content = null ) {

    $a = shortcode_atts( array(
        'url' => '',
        'title'  =>  ''
    ), $atts );

    return '<a href="' . esc_attr($a['url']) . '" class="footer_shortcody_linker">' . esc_attr($a['title']) . '</a>' ;

}

add_shortcode( 'rt-link', 'footer_link_shortcode' );
```



## Final Result

A fully functioning , fully responsive WordPress website developed as per the original PSD file provided :) 

Please let me know if you have any questions.



# Thank you
