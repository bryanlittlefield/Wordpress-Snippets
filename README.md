Wordpress-Snippets
==================

A collection of usefull snippets



###Combining Two Post Queries 
```php
<?php
$occasion_args = array(
     'numberposts' => 100,
     'post_type' => 'product',
     'orderby'    => 'meta_value',
     'meta_query' => array(

          array(
               'key' => 'from',
               'value' => 20140701,
               'compare' => '>='
          ),
          array(
               'key' => 'to',
               'value' => 20140731,
               'compare' => '<='
          )
     )
);

//First query
$query1 = get_posts($occasion_args);

//Second query
$query2 = get_posts(array(
          'post_type' => 'product',
          'post_status' => 'publish',
          'meta_key' => 'seasonal_cupcake',
          'meta_value' => 'no'          
          ));

$mergedposts = array_merge( $query1, $query2 ); //combine the two queries together

$postids = array();

foreach( $mergedposts as $item ) {

     $postids[] = $item->ID; // Get Id's for the posts

}

$uniqueposts = array_unique($postids); //Remove duplicate post ids

// Get Post Objects from Unique Queried Post ID's
$posts = get_posts(array(
          'post__in' => $uniqueposts, 
          'post_type' => 'product',
          'post_status' => 'publish',
          ));
?>

<!-- Loop Through the Post Objects -->
<?php foreach( $posts as $post ) : ?>

          <!-- Setup Data so we can run through as a loop -->
          <?php setup_postdata($post) ?>
          
          <!-- Setup WC_Product to make the object accessible a woo product -->
          <?php $product = new WC_Product( get_the_ID()); ?>

          <?php the_title(); ?>

          <?php print_r($product) ?>

<?php endforeach; ?>   
```
