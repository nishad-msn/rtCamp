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
   
   
   
