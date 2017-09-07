# php7-ext-trie-filter

```
php7 版本的敏感词过滤
官方链接：
https://github.com/wulijun/php-ext-trie-filter
```
参考：
https://yq.aliyun.com/articles/59016

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




php extension for spam word filter based on Double-Array Trie tree, it can detect if a spam word exists in a text message.

关键词过滤扩展，用于检查一段文本中是否出现敏感词，基于Double-Array Trie 树实现。

## 升级历史

### 2017-9-7
这个是PHP7版本的。

## 依赖库

[libdatrie-0.2.4 or later](http://linux.thai.net/~thep/datrie/datrie.html)

## 安装步骤

下面的$LIB_PATH为依赖库安装目录，$INSTALL_PHP_PATH为PHP5安装目录。

### 安装libdatrie
    $ tar zxvf libdatrie-0.2.4.tar.gz
    $ cd libdatrie-0.2.4
    $ make clean
    $ ./configure --prefix=$LIB_PATH
    $ make
    $ make install

### 安装扩展   
    $ $INSTALL_PHP_PATH/bin/phpize
    $ ./configure --with-php-config=$INSTALL_PHP_PATH/bin/php-config --with-trie_filter=$LIB_PATH
    $ make
    $ make install

然后修改php.ini，增加一行：extension=trie_filter.so，然后重启PHP。

## 使用示例
	<?php
	$arrWord = array('word1', 'word2', 'word3');
	$resTrie = trie_filter_new(); //create an empty trie tree
	foreach ($arrWord as $k => $v) {
    	trie_filter_store($resTrie, $v);
	}
	trie_filter_save($resTrie, __DIR__ . '/blackword.tree');

	$resTrie = trie_filter_load(__DIR__ . '/blackword.tree');

	$strContent = 'hello word2 word1';
	$arrRet = trie_filter_search($resTrie, $strContent);
	print_r($arrRet); //Array(0 => 6, 1 => 5)
	echo substr($strContent, $arrRet[0], $arrRet[1]); //word2
	$arrRet = trie_filter_search_all($resTrie, $strContent);
	print_r($arrRet); //Array(0 => Array(0 => 6, 1 => 5), 1 => Array(0 => 12, 1 => 5))
	

	$arrRet = trie_filter_search($resTrie, 'hello word');
	print_r($arrRet); //Array()

	trie_filter_free($resTrie);

# PHP版本

PHP 5.2 or later.

Windows is not support until now.

## License

Apache License 2.0

## 致谢

本项目是在[用于检测敏感词的 PHP 扩展](http://blog.anbutu.com/php/php-ext-trie-filter)的基础上改写的。

