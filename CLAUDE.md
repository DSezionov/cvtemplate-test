# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

# SevenPro CV Generator

## Що це
Веб-додаток на GitHub Pages для генерації CV рекрутерами.
Файли: index.html (увесь додаток), template.docx (шаблон для DOCX)

## Як працює
- Форма → live preview → Download DOCX / PDF
- DOCX: docxtemplater заповнює template.docx плейсхолдерами
- PDF: html2pdf.js рендерить preview div

## Стек
Vanilla JS, без фреймворків. html2pdf.js, docxtemplater, pizzip (CDN)

## Шрифт
Montserrat скрізь (Google Fonts)

## Репозиторій
https://github.com/dsezionov/cvtemplate-test
GitHub Pages: https://dsezionov.github.io/cvtemplate-test/
