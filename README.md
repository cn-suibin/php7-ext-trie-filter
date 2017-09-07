# php7-ext-trie-filter



```
/* PHP5
static void php_trie_filter_dtor(zend_rsrc_list_entry  *rsrc TSRMLS_DC)
{
	Trie *trie = (Trie *)rsrc->ptr;
	trie_free(trie);
}

*/
//PHP7
static void php_trie_filter_dtor(zend_resource  *rsrc TSRMLS_DC)
{
	Trie *trie = (Trie *)rsrc->ptr;
	trie_free(trie);
}
```


```
修改位置：
//PHP5
//	ZEND_FETCH_RESOURCE(trie, Trie *, &trie_resource, -1, PHP_TRIE_FILTER_RES_NAME, le_trie_filter);
//PHP7
	zend_fetch_resource(Z_RES_P(trie_resource),PHP_TRIE_FILTER_RES_NAME, le_trie_filter);
```
