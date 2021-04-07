If you are not too keen on installing an extra plugin to showcase popular posts (say, you have limited server memory or disk space), follow this snippet.

Paste the following in functions.php:

```
function count_post_visits() {
    if( is_single() ) {
        global $post;
        $views = get_post_meta( $post->ID, 'my_post_viewed', true );
        if( $views == '' ) {
            update_post_meta( $post->ID, 'my_post_viewed', '1' );   
        } else {
            $views_no = intval( $views );
            update_post_meta( $post->ID, 'my_post_viewed', ++$views_no );
        }
    }
}
add_action( 'wp_head', 'count_post_visits' );
```

Thereafter, paste the following wherever in your template files that you wish to display the popular posts:

```
$popular_posts_args = array(
    'posts_per_page' => 3,
    'meta_key' => 'my_post_viewed',
    'orderby' => 'meta_value_num',
    'order'=> 'DESC'
);
$popular_posts_loop = new WP_Query( $popular_posts_args );
  while( $popular_posts_loop->have_posts() ):
    $popular_posts_loop->the_post();
    // Loop continues
endwhile;
wp_reset_query();
```