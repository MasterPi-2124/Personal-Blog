---
# Name of the article
title: "{{ replace .Name "-" " " | title }}"

# Quick description
description: Lorem ipsum dot sit amet

# Author of the article
author: Master Pi

# Date created
date: {{ .Date }}

# Article's tags
tags: 


# Article's categories: Blog, Project or Guideline
categories:


# Useful to link articles together for "See also" part
series: 

# is Math included? Default: false
math: false

# Cover image of the article
image: 

# Is drafted? If true, the article will not be shown on website. Default: true, change to "false" when finish
draft: true

# Appears as the tail of the output URL.
slug: "{{ .Name | lower }}"
---

