<?php
if ( ! defined('BASEPATH')) exit('No direct script access allowed'); 
/**
 * ---------------------------------------------------------------------------------------
 * 
 * A meta tag generation library for CodeIgniter.
 * 
 * Copyright (c) 2009 Per Sikker Hansen
 * Copyright (c) 2013 Daniel Carbone
 * 
 * Permission is hereby granted, free of charge, to any person obtaining a copy
 * of this software and associated documentation files (the "Software"), to deal
 * in the Software without restriction, including without limitation the rights
 * to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
 * copies of the Software, and to permit persons to whom the Software is
 * furnished to do so, subject to the following conditions:
 * 
 * The above copyright notice and this permission notice shall be included in
 * all copies or substantial portions of the Software.
 * 
 * THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
 * IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
 * FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
 * AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
 * LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
 * OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN
 * THE SOFTWARE.
 * 
 * Originally created by:
 * @author Per Sikker Hansen <per@sikker-hansen.dk>
 *
 * Modified By: Daniel Carbone (daniel.p.carbone@gmail.com)
 *
 * Removed the differences in output between HTMl and XHTML as they were pointless.
 * Redefined the class variables and removed DOCTYPE completely as this library
 * does not hande doctype output anyway
 * Updated to take advantage of PHP5 class structure and some other minor
 * modifications
 */

class Meta_tags
{
    protected static $_CI;
    protected static $tags = array();
    protected static $keywords = array();
    protected static $robots = array();
    
    private static $_instance = NULL;
    
    public static function _init(Array $config = array())
    {
        if (is_null(static::$_instance))
        {
            static::$_instance = new self($config);
        }
        return static::$_instance;
    }
    
    /**
     * Class constructor with optional parameter, which calls the initialize() method
     * @param $config array optional array containing configuration
     */
    private function __construct(Array $config = array())
    {
        self::$_CI =& get_instance();
        
        if(count($config) < 1)
        {
            $config = self::$_CI->config->item('meta_tags');
            if(!$config)
                $config = array();
        }  

        if(isset($config['tags']))
            self::$tags = $config['tags'];
        
        if(isset($config['keywords']))
            self::$keywords = $config['keywords'];
            
        if(isset($config['robots']))
            self::$robots = $config['robots'];
    }
    
    /**
     * Sets a meta tag with name and content
     * @param $name string
     * @param $content string
     */
    public static function set_meta_tag($name, $content)
    {
        self::$tags[$name] = $content;
    }
    
    /**
     * @name add_meta_tag_value
     * @param String $name
     * @param Mixed $content
     */
    public static function add_meta_tag_value($name, $content)
    {
        if ((!is_string($name) && !is_array($name)) || (!is_string($content) && !is_array($content))) return false;
        
        if ($cur = self::get_meta_tag($name))
        {
            if (is_string($cur) && strlen($cur) > 0)
            {
                if (is_array($content))
                {
                    $cur .= implode(" ", $content);
                }
                elseif (is_string($content))
                {
                    $cur .= $content;
                }
                self::set_meta_tag($name, $cur);
            }
            else 
            {
                self::set_meta_tag($name, $content);
            }
        }
        else
        {
            self::set_meta_tag($name, $content);
        }
    }
    
    /**
     * @name get_meta_tag
     * @access public
     * @param String $name
     * @abstract returns either current value for meta tag or false
     */
    public static function get_meta_tag($name)
    {
        return isset(self::$tags[$name]) ? self::$tags[$name] : false;
    }
    
    /**
     * Removes a meta tag
     * @param $name string name of the tag
     */
    public static function unset_meta_tag($name)
    {
        unset(self::$tags[$name]);
    }
    
    /**
     * Adds a unit to the keyword array
     * @param $keyword string
     */
    public static function add_keyword($keyword)
    {
        $this->remove_keyword($keyword);
        self::$keywords[] = $keyword;
    }
    
    /**
     * Searches the keywords array and removes the given keyword
     * @param $keyword string
     */
    public static function remove_keyword($keyword)
    {
        $this->_search_and_remove($keyword, self::$tags);
    }
    
    /**
     * Adds a rule to the robots array
     * @param $rule string
     */
    public static function add_robots_rule($rule)
    {
        self::remove_robots_rule($rule);
        self::$robots[] = $rule;
    }
    
    /**
     * Searches the robots array and removes the given rule
     * @param $rule string
     */
    public static function remove_robots_rule($rule)
    {
        self::_search_and_remove($rule, self::$robots);
    }
    
    /**
     * Library-only function for searching and removing
     * @param $needle string
     * @param $haystack array
     */
    private static function _search_and_remove($needle, $haystack)
    {
        $key = array_search($needle, $haystack);
        if($key)
        {
            unset($haystack[$key]);
        }
    }
    
    /**
     * Passes the contained data to private functions for processing
     * @return string the compiled meta tags for insertion into your view
     */
    public static function generate_meta_tags()
    {
        $output = "\n";
        
        if(count(self::$robots) > 0)
        {
            $output .= '<meta name="robots" content="'.implode(',', self::$robots).'" />'."\n";
        }
        
        if(count(self::$tags) > 0)
        {
            foreach(self::$tags as $name=>$content)
            {
                $output .= '<meta name="'.$name.'" content="'.$content.'" />'."\n";
            }
        }
        if(count(self::$keywords) > 0)
        {
            $output .= '<meta name="keywords" content="'.implode(',', self::$keywords).'" />'."\n";
        }
        return $output;
    }
    
}

/* End of file meta_tags.php */
/* Location: ./application/libraries/meta_tags.php */
