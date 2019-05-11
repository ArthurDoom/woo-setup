# Woocommerce Theme Setup
Some basics to adding Woocommerce support to your Wordpress theme

## Adding theme support for woocommerce
To declare woocommerce support add the following code to your functions file.

```
function ew_s_s_woocommerce_setup() {
	add_theme_support( 'woocommerce' ); //Declares support for woocommerce
	add_theme_support( 'wc-product-gallery-zoom' ); //The zoom Gallery
	add_theme_support( 'wc-product-gallery-lightbox' ); //The product Light Box
	add_theme_support( 'wc-product-gallery-slider' ); //The gallery Slider
}
add_action( 'after_setup_theme', 'ew_s_s_woocommerce_setup' );
```

## Adding woo font styles to your theme 

```
unction ew_s_s_woocommerce_scripts() {
	wp_enqueue_style( 'ewew_s_s_s-woocommerce-style', get_template_directory_uri() . '/woocommerce.css' );

	$font_path   = WC()->plugin_url() . '/assets/fonts/';
	$inline_font = '@font-face {
			font-family: "star";
			src: url("' . $font_path . 'star.eot");
			src: url("' . $font_path . 'star.eot?#iefix") format("embedded-opentype"),
				url("' . $font_path . 'star.woff") format("woff"),
				url("' . $font_path . 'star.ttf") format("truetype"),
				url("' . $font_path . 'star.svg#star") format("svg");
			font-weight: normal;
			font-style: normal;
		}';

	wp_add_inline_style( 'ewew_s_s_s-woocommerce-style', $inline_font );
}
add_action( 'wp_enqueue_scripts', 'ew_s_s_woocommerce_scripts' );
```

## Adding the class woocommerce-active to the body
Usefull when you have a theme that can operate without woocommerce. You can target styles based on the body class woocommerce-active 

```
function ew_s_s_woocommerce_active_body_class( $classes ) {
	$classes[] = 'woocommerce-active';

	return $classes;
}
add_filter( 'body_class', 'ew_s_s_woocommerce_active_body_class' );
```

## Replacing the default Woocommerce wrapper

```
/**
 * Remove default WooCommerce wrapper.
 */
remove_action( 'woocommerce_before_main_content', 'woocommerce_output_content_wrapper', 10 );
remove_action( 'woocommerce_after_main_content', 'woocommerce_output_content_wrapper_end', 10 );

if ( ! function_exists( 'ew_s_s_woocommerce_wrapper_before' ) ) {
	/**
	 * Before Content.
	 *
	 * Wraps all WooCommerce content in wrappers which match the theme markup.
	 *
	 * @return void
	 */
	function ew_s_s_woocommerce_wrapper_before() {
		?>
		<div id="primary" class="content-area">
			<main id="main" class="site-main" role="main">
		<?php
	}
}
add_action( 'woocommerce_before_main_content', 'ew_s_s_woocommerce_wrapper_before' );

if ( ! function_exists( 'ew_s_s_woocommerce_wrapper_after' ) ) {
	/**
	 * After Content.
	 *
	 * Closes the wrapping divs.
	 *
	 * @return void
	 */
	function ew_s_s_woocommerce_wrapper_after() {
		?>
			</main><!-- #main -->
		</div><!-- #primary -->
		<?php
	}
}
add_action( 'woocommerce_after_main_content', 'ew_s_s_woocommerce_wrapper_after' );
```
