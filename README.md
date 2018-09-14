<h1>How to add custom taxonomy to post</h1>
<pre>
$my_taxon_name  = 'Locaions';
$my_post_types  = array('event');
//REGISTER CUSTOM TAXONOMY ( http://codex.wordpress.org/Function_Reference/register_taxonomy )
//If you aim to register HIERARCHICAL(Parent-ed) post type, read this warning: https://codex.wordpress.org/Function_Reference/register_post_type#hierarchical
add_action( 'init', 'my_f32' ); function my_f32(){ 
    register_taxonomy( $GLOBALS['my_taxon_name'], array(), 
        array( 
			'label'=>$GLOBALS['my_taxon_name'],
			'public'=>true,
			'show_ui'=>true,
			'show_admin_column'=>true,
			'query_var'=>true,
            'hierarchical'=>true,
			'rewrite'=>array(
				'with_front'=>true,
				'hierarchical'=>true
			),  
        ));
}
//REGISTER CUSTOM POST TYPE ( http://codex.wordpress.org/Function_Reference/register_post_type )
add_action( 'init', 'myf_63' );function myf_63() { 

    foreach ($GLOBALS['my_post_types'] as $each_Type)       {
            register_post_type( $each_Type, 
                array( 
                    'label'=>$each_Type,     'labels' => array('name'=>$each_Type.' pagess', 'singular_name'=>$each_Type.' page'),        'public' => true,   'publicly_queryable'=> true,      'show_ui'=>true,      'capability_type' => 'post',      'has_archive' => true,      'query_var'=> true,     'can_export' => true,                   //'exclude_from_search' => false,     'show_in_nav_menus' => true,  'show_in_menu' => 'edit.php?post_type=page',//true,     'menu_position' => 5,
                    'hierarchical' =>true,
                    'supports' =>array( 'page-attributes', 'title', 'editor', 'thumbnail' ), 
                    'rewrite' => array('with_front'=>true, ),     //    'rewrite' => array("ep_mask"=>EP_PERMALINK ...) OR    'permalink_epmask'=>EP_PERMALINK, 
                ));

            register_taxonomy_for_object_type('category',$each_Type);       //standard categories
            register_taxonomy_for_object_type($GLOBALS['my_taxon_name'] ,$each_Type);   //Custom categories
    }
}
</pre>
