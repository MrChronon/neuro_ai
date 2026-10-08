# NeuroAI Learning Protocol / Протокол обучения NeuroAI

## Purpose / Цель

This project is developed as a learning-to-research track: theory, visual understanding, real data, code, reproducible analysis, and portfolio growth should evolve together.

Этот проект развивается как путь от обучения к исследованию: теория, визуальное понимание, реальные данные, код, воспроизводимый анализ и портфолио должны развиваться вместе.

## Working environment / Рабочая среда

Primary workspace: ChatGPT Work.

Основная рабочая среда: ChatGPT Work.

Use Work for:
- maintaining project documents and notes;
- creating and revising research artifacts;
- accumulating a reusable project knowledge base;
- working with files and datasets;
- preparing materials that are later committed to GitHub;
- keeping the project coherent across many sessions.

Используем Work для:
- ведения документов и заметок проекта;
- создания и доработки исследовательских материалов;
- накопления переиспользуемой базы знаний;
- работы с файлами и датасетами;
- подготовки материалов для последующей выгрузки в GitHub;
- сохранения целостности проекта между сессиями.

GitHub remains the canonical public project repository for code, notebooks, documentation, figures, and selected results. Raw neuroimaging data are not committed.

GitHub остаётся основным публичным репозиторием проекта для кода, notebooks, документации, иллюстраций и выбранных результатов. Raw neuroimaging data в GitHub не загружаются.

## Learning format / Формат обучения

Each topic should normally contain five parts:

1. Concept / Концепция
   - explain only what is needed now;
   - connect it to statistics, psychology, Data Science or ML where useful;
   - the lecture text remains the primary learning material and should not be replaced by an infographic or summary poster.

2. Visual explanation / Наглядное объяснение
   - every important new concept should be accompanied by a diagram, illustration, annotated image, plot, or other visual representation when this improves understanding;
   - visuals should explain the mechanism, structure, spatial relation, temporal process, or data representation rather than decorate the lesson;
   - visuals should be embedded directly between the relevant conceptual blocks, not collected into one large poster by default;
   - one visual should usually explain one idea or one small cluster of closely related ideas;
   - a large summary poster is optional and should only be created when explicitly useful as a recap.

3. Practice / Практика
   - use real neuroimaging data as early as possible;
   - first explain what each action does and why;
   - then the learner writes or modifies the code independently.

4. Understanding check / Проверка понимания
   - short questions or interpretation tasks;
   - identify misconceptions before moving on.

5. Project progress / Прогресс проекта
   - record what was learned, created, visualized, or analyzed;
   - add durable artifacts to Work and, when appropriate, GitHub.

## Visual learning rule / Правило визуального обучения

From now on, visual material is a default part of the project.

С этого момента наглядный материал является стандартной частью проекта.

Preferred visual types:
- mechanism diagrams;
- brain/anatomy illustrations;
- voxel/volume and coordinate diagrams;
- experimental timelines;
- BIDS directory diagrams;
- signal and time-series plots;
- annotated screenshots of real data;
- design matrices and GLM visualizations;
- later: EEG/MEG sensor maps, activation maps, RDMs, decoding plots.

When a real scientific figure is used, preserve attribution and source information. When an original explanatory illustration is created for the project, store it in `figures/` when it is useful for documentation or the portfolio.

## Pace / Темп

Current pace:
- approximately 3-4 hours per week;
- small steps;
- theory and practice have equal priority;
- mathematics follows: intuition -> formula -> small worked example -> Python -> interpretation;
- do not move to a more complex stage while a foundational gap remains.

## Current milestone / Текущая контрольная точка

The learner should be able to:
- understand the basic experimental logic of the Wakeman & Henson dataset;
- distinguish MRI, fMRI, EEG and MEG at a conceptual level;
- understand BOLD, voxel, volume and temporal sampling;
- navigate the BIDS structure;
- load a real NIfTI file in Python;
- inspect its dimensions and metadata;
- visualize a real brain image and explain what is being displayed.

Only after this milestone do we move to events, design matrices and GLM.
