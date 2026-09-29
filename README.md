# Contact tracing with missing information: an interactive toy model

A single-file, browser-based demo of how **incomplete contact tracing** changes the course of an emerging epidemic. A few hundred synthetic residents live, study, work and socialise across four districts while an infection spreads and investigators trace confirmed cases. You can make the tracing leaky in two different ways and watch the outbreak and its transmission tree respond.

> **Created by Claude Opus 5.5 (Anthropic).**
> This is an independent, simplified reimplementation of the model in:
>
> **Min-Kyung Chae, Woo-Sik Son and Sang Hoon Lee**, "Evaluating the impact of incomplete contact tracing data on urban epidemic dynamics," *Scientific Reports* (2026). https://doi.org/10.1038/s41598-026-66867-6
>
> It is not affiliated with or endorsed by the authors. The authors' original simulation code is at https://github.com/MkChae/ABM_CT and their input data at https://doi.org/10.6084/m9.figshare.31142995.

## What you can do

- **Choose the response:** no intervention, test-and-isolate only, or full contact tracing.
- **Add information loss:**
  - *Untraced cases* (infector omission, IO): a confirmed case is isolated, but their movements are never reconstructed, so none of their contacts are found.
  - *Missed contacts* (contact omission, CO): identified contacts never receive a test-and-quarantine notice. Choose **Friends only** (the paper's selective scenario, SCO) or **All networks** (uniform scenario, UCO).
  - *Missed community encounters*: fixed at 50% in the paper, adjustable here.
- **Tune tracing:** share of symptomatic people who get tested, delay from confirmation to tracing, quarantine length before the release test.
- **Tune the pathogen and town:** transmissibility, number of initially exposed people, population size (300 / 500 / 800), or generate a new town.
- **Watch the network:** people coloured by state, quarantine and isolation rings, a `?` on cases that were never traced, and the growing transmission tree. Hover for details on any person; tap a susceptible person to expose them.
- **Read the live panels:** people infected, transmission depth (longest infection chain, the paper's network diameter), confirmed cases, quarantined and missed contacts, a time series by state, the share of infections by setting, and an investigation log.
- **Find the breaking point:** a sweep repeats the outbreak 20–80 times per omission rate and plots mean infections and transmission depth with the middle-90% band. Run IO, SCO and UCO to overlay them.

## The model

| Component | Setting in this demo | Source in the paper |
|---|---|---|
| Contact layers | Household (daily), classroom and workplace (weekdays), friends and local community (each 1/7 chance per day) | Methods, Table S1 |
| Friendships | Age-homophilic Barabási–Albert network within decade cohorts, h = 0.9 | Methods, S1.6 |
| Commuting | ~28% of workers and ~9% of students work or study outside their home district | S1.7 |
| Latent period | Gamma (shape 1.926, scale 1.775) | Table 1 |
| Infectiousness | Starts 2 days before symptoms; viral-shedding profile gamma with mean 3.067, sd 2.109; individual factor ξ ~ Gamma(1, 0.5) | Table 1 |
| Asymptomatic share | 20% | Table 1 |
| Infectious period | 8 days | Table 1 |
| Transmission per contact | P = 1 − exp(−β · t · φ), with contact duration t drawn from the Korean survey distributions per setting | Eq. 1, S3 |
| Self-testing | 50% of symptomatic people self-quarantine for 1 day and get tested | Table 1 |
| Isolation | 7 days after a positive test | Table 1 |
| Tracing | Contacts from the previous 10 days are quarantined, tested on release, and positives start a new round | Contact Tracing section |

## Differences from the paper

- **Scale.** The paper simulates 9.5 million agents (Seoul) and 3.3 million (Busan) built from census microdata. This demo uses a few hundred synthetic agents, so the paper's thresholds (about 4% IO in Seoul and 10% in Busan) do not transfer. Expect a gradual rise rather than a sharp transition.
- **Transmissibility** β is a free slider; its default was chosen so that tracing contains most outbreaks with no information loss.
- **Classrooms** group three-year age bands rather than a single age; **friend gatherings** are pairwise rather than group events.
- **Testing** detects an infection only once it is at least two days old; the 10-day trace window is an assumption of this demo.
- Tracing delay and simultaneous omission are available as sliders, echoing the paper's supplementary analyses (S5.1, S5.2), but are not calibrated to them.

## What to look for

- With the same omission rate, **untraced cases hurt far more than missed contacts**: one untraced case hides an entire downstream branch, while one missed contact hides a single link.
- As infector omission rises, the **transmission depth grows**, meaning chains run deeper into the population before they are cut.
- A **one- or two-day tracing delay** overwhelms the effect of completeness, as in the paper's supplementary analysis.

## File

- `contact_tracing_demo.html`: the whole app (HTML, CSS and JavaScript in one file, no external scripts).

## Credits and licence notes

- Demo created by **Claude Opus 5.5** (Anthropic).
- Underlying model and parameters: **Min-Kyung Chae** (Sejong University), **Woo-Sik Son** (National Institute for Mathematical Sciences) and **Sang Hoon Lee** (Gyeongsang National University).
- The paper is published under CC BY-NC-ND 4.0. This repository contains no text or figures from the paper, only a reimplementation using its published parameter values. Please cite the original article if you use this demo in teaching or talks.
