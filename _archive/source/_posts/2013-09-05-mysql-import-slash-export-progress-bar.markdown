---
layout: post
title: "MySQL Import/Export Progress Bar"
date: 2013-09-05 22:05
updated: 2013-09-05 22:05
comments: true
categories: [unix, mysql]
---

MySQL command line progress can be monitored using the terminal-based "Pipe Viewer" `pv`.

## Import

	pv /path/to/sqlfile.sql | mysql -uUSERNAME -pPASSWORD -D DATABASE_NAME

## Export

We need to estimate the file size of our export to get an accurate reading.  This can be done via the information schema:

	SELECT
		Data_BB / POWER(1024,1) Data_KB,
		Data_BB / POWER(1024,2) Data_MB,
		Data_BB / POWER(1024,3) Data_GB
	FROM (
		SELECT SUM(data_length) Data_BB
		FROM information_schema.tables
		WHERE table_schema IN ('DATABASE_NAME')
	) A;

We then use the estimated size to track export progress: 

	mysqldump -uUSERNAME -pPASSWORD DATABASE_NAME | pv -s 9999M > DATABASE_NAME.sql	

Beer Break!

