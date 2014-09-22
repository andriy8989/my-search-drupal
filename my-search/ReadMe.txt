For example Mysearch module will create create 5 node of Page content type, each containing "example" word.

So you may type any word at the end of the url http://your.domain.com/mysearch/your_search_word to search all the nodes containing "your_search_word".

I made ​​the following changes to the file "mysearch.module" that earned module

/////////////////////////////////////////////////////////////////////////////////////////////////

In mysearch_searchpage function:
----------------
  replace code
-------------- 
  $query = "
     SELECT nid
     FROM node_revisions
     WHERE body LIKE '%$searchterm%'
     ";
  $search_results = db_query($query); 
  $search_results = "<h2>Search for $searchterm</h2><ul>";
  foreach($get_node_result as $record)) {
    $node = node_load($record->nid);
    $search_results .= '<li><a href="/node/'
     . $node->nid . '" title="' . $node->title . '">'
     . $node->title . '</a><!-- debugging output: '
     . print_r($node) . '  --><\li>';
  }
  return $search_results;
----------------
   by code
----------------
  $query = "
     SELECT revision_id
     FROM field_data_body
     WHERE body_value LIKE '%$searchterm%'
     ";
  $search_results = db_query($query);

  $result = '<h2>Search for word <b>' . $searchterm . '</b></h2>';
  
    $result .= t('<p>Here you can see the searh result by the word <b>' . $searchterm . '</b></p>');
    $result .= t('<p>You can start new word searching by setting address line to <a>http://your.domain.com/mysearch/WORD_TO_SEARCH</a></p>');
    $result .= '<ul>';
    foreach($search_results as $record) {
       $node = node_load($record->revision_id);
       $result .= '<li><a href="/node/' . $node->nid . '" title="' . $node->title . '">' . $node->title . '</li>';
     }
    $result .= '</ul>';	

  return $result;
----------------
Here was removed code mistakes in database query, analising of query and print results. 
Also it was added axplaining text to top of the page.
---------------------------------------------------------------------------------------------  
It was the module Help page added.
---------------------------------------------------------------------------------------------  
It was mysearch.install file added to create 5 nodes for showing module works.
All the created nodes contain "example" word just for example.
---------------------------------------------------------------------------------------------
In hook_menu:
  $items['mysearch/%'] 
this is for user may search any word.
---------------------------------------------------------------------------------------------  