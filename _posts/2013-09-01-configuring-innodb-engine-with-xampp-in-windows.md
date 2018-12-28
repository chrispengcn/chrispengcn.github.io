---
ID: 257
post_title: >
  Configuring INNODB Engine with XAMPP in
  Windows
author: chris
post_excerpt: ""
layout: post
permalink: >
  http://hss5.com/2013/09/01/configuring-innodb-engine-with-xampp-in-windows/
published: true
post_date: 2013-09-01 07:16:11
---
<p>InnoDB MySQL database storage engine is not enabled in the my.cnf<br>configuration file of XAMPP.<br>I have configured successfully Innodb engine with XAMPP latest version.<br>To enable the support of MySQL server on InnoDB storage engine, locate<br>the “my.cnf” config file (normally in /installation_path/xampp/mysql/<br>bin/ directory), and edit the my.cnf with any text editor.<br>Search and locate each of the following lines (except the lines in<br>italic where they’re comments):<br>- Comment the following line to unskip and use InnoDB<br>skip-innodb<br>- Uncomment the following options for InnoDB database if you are<br>using InnoDB tables.<br>#innodb_data_home_dir = C:/xampp/xampp/mysql/data/<br>#innodb_data_file_path = ibdata1:10M:autoextend<br>#innodb_log_group_home_dir = C:/xampp/xampp/mysql/data/<br>#innodb_log_arch_dir = C:/xampp/xampp/mysql/data/<br>- Uncomment the lines and set innodb_buffer_pool_size up to 50% -<br>80% of RAM for optimization of InnoDB databases, try not to memory<br>usage too high.<br>#set-variable = innodb_buffer_pool_size=16M<br>#set-variable = innodb_additional_mem_pool_size=2M<br>- Uncomment the lines and set innodb_log_file_size to 25% of<br>InnoDB buffer pool size for optimisation.<br>#set-variable = innodb_log_file_size=5M<br>#set-variable = innodb_log_buffer_size=8M<br>#innodb_flush_log_at_trx_commit=1<br>#set-variable = innodb_lock_wait_timeout=50<br>After modification, the code for each lines should look like this:<br># skip-innodb<br>innodb_data_home_dir = C:/xampp/xampp/mysql/data/<br>innodb_data_file_path = ibdata1:10M:autoextend<br>innodb_log_group_home_dir = C:/xampp/xampp/mysql/data/<br>innodb_log_arch_dir = C:/xampp/xampp/mysql/data/<br>set-variable = innodb_buffer_pool_size=16M<br>set-variable = innodb_additional_mem_pool_size=2M<br>set-variable = innodb_log_file_size=5M<br>set-variable = innodb_log_buffer_size=8M<br>innodb_flush_log_at_trx_commit=1<br>set-variable = innodb_lock_wait_timeout=50 <p>&nbsp; <p><a title="http://tapos.wordpress.com/2008/01/03/configuring-innodb-engine-with-xampp-in-windows/" href="http://tapos.wordpress.com/2008/01/03/configuring-innodb-engine-with-xampp-in-windows/">http://tapos.wordpress.com/2008/01/03/configuring-innodb-engine-with-xampp-in-windows/</a></p>