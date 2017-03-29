# How-to-dynamic-isotop-in-wordpress
Here I explaine How you can dynamic mixitup Or isotop in wordpress


1st : Enqueue your Js

	wp_enqueue_script( 'centric-isotope-js', get_bloginfo( 'stylesheet_directory' ) . '/js/isotope.pkgd.min.js', array( 'jquery' ), '', true );

2nd : Create custome post and taxonomy Like

	/****************************************************
	* Register course post type
	*****************************************************/
	$labels = array(
		'name'                => _x( 'Course', 'Post Type General Name', 'centric-pro' ),
		'singular_name'       => _x( 'Course', 'Post Type Singular Name', 'centric-pro' ),
		'all_items'           => __( 'All Courses', 'centric-pro' ),
		'view_item'           => __( 'View Course', 'centric-pro' ),
		'add_new_item'        => __( 'Add New Course', 'centric-pro' ),
		'add_new'             => __( 'Add New Course ', 'centric-pro' ),
		'edit_item'           => __( 'Edit Course', 'centric-pro' ),
		'update_item'         => __( 'Update Course', 'centric-pro' ),
		'search_items'        => __( 'Search Courses', 'centric-pro' ),
		'not_found'           => __( 'Course Not found', 'centric-pro' ),
		'not_found_in_trash'  => __( 'Course Not found in Trash', 'centric-pro' ),
	);
	$args = array(
		'label'               => __( 'Course', 'centric-pro' ),
		'labels'              => $labels,
		'supports'            => array( 'title', 'editor', 'excerpt', 'thumbnail', 'custom-fields', 'page-attributes', ),
		'public'              => true,
		'show_in_menu'        => true,
		'menu_position'       => 10,
		'menu_icon'           => 'dashicons-smiley',
		'can_export'          => true,
		'has_archive'         => true,
		'exclude_from_search' => false,
		'rewrite'             => array( 'slug' => 'course' ),
	);
	register_post_type( 'course', $args );

	/****************************************************
	* Register custom taxonomy for  post type
	*****************************************************/

	$labels = array(
		'name'					=> _x( 'Course Catergories', 'Taxonomy General Name', 'centric-pro' ),
		'singular_name'			=> _x( 'Course Category', 'Taxonomy Singular Name', 'centric-pro' ),
		'add_new_item'			=> __( 'Add New Course Category', 'centric-pro' ),
		'edit_item'				=> __( 'Edit Course Category', 'centric-pro' ),
		'update_item'			=> __( 'Update Course Category', 'centric-pro' ),
		'add_or_remove_items'	=> __( 'Add or remove Course Category', 'centric-pro' ),
	);

	$args = array(
		'labels'            => $labels,
		'public'            => true,
		'hierarchical'      => true,
		'rewrite'             => array( 'slug' => 'course-category' ),
	);

	register_taxonomy( 'course-category', array( 'course' ), $args );

3rd: Now Go to your Template Pard/ your code file where you start loop

	<section id="isotop_area">
	<div class="container">
	<div class="row">
	<div class="col-md-12">
	<h1>Isotope</h1>
	<?php $filters = get_terms('course-category'); //"course-category" Here taxonomy name
	?>
	<div id="filters" class="button-group">  
	<button class="button is-checked" data-filter="*">show all</button>
	<?php foreach ($filters as $filter) { ?>
	<button class="button" data-filter=".<?php echo $filter->slug; ?>"><?php echo $filter->name; ?></button>
	<?php } ?>
	</div>
	<div class="grid">
	<?php $args = array (
	'post_type' => 'course',
	'posts_per_page' => -1,
	'category_name' => '',
	);
	$course_query = new WP_Query($args);
	if ( $course_query->have_posts() ) :
	?>
	<?php while ( $course_query->have_posts() ) : $course_query->the_post(); ?>
	<div class="llms-loop-item-content" style="height: 378px;">
	<a class="llms-loop-link" href="http://smarty.ncreatives.com/course/course-3/">
	<div class="llms-featured-image wp-post-image">
		<?php the_post_thumbnail( '' ); ?>
	</div>
	<div class="course-meta"><span class="level"></span><span class="runtime"></span></div>
	<h4 class="llms-loop-title"><?php the_title();?></h4>

	<footer class="llms-loop-item-footer">
		<p><?php the_content(); ?></p>
	</footer>
	</a>
	</div>
	</div>
	<?php endwhile; ?>
	<?php endif; wp_reset_query(); ?>
	</div>
	</div>
	</div>
	</section>


4th: Put this code in functions.php file

	// Function 'add_portfolio_filter_into_post_class' starts
	function add_portfolio_filter_into_post_class( $post_classes) {

		$filters = get_the_terms( get_the_ID(), 'course-category' ); //"course-category" Here taxonomy name

		foreach ($filters as $filter) {
			$post_classes[] = $filter->slug;
		}
		return $post_classes;
	} 
	// Function 'add_portfolio_filter_into_post_class' ends


	add_action( 'post_class', 'add_portfolio_filter_into_post_class' );