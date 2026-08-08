<script setup>
import { ref, onMounted, onUnmounted } from 'vue'

const activeSection = ref('home')
const navCollapsed = ref(true)

const sections = [
  { id: 'home', label: 'Home' },
  { id: 'program', label: 'Schedule' },
  { id: 'keynote', label: 'Speakers' },
  { id: 'accepted-papers', label: 'Papers' },
  { id: 'submission', label: 'Call for Papers' },
  { id: 'dates', label: 'Important Dates' },
  { id: 'organizers', label: 'Committee' },
]

const topics = [
  { icon: 'bi-cpu', title: 'Mining Agent-Generated Artifacts', desc: 'Analyzing code, PRs, and commits produced by AI Teammates at scale.' },
  { icon: 'bi-people', title: 'Human-Agent Collaboration & Alignment', desc: 'How developers and AI agents work together effectively.' },
  { icon: 'bi-shield-check', title: 'Trustworthiness & Reliability', desc: 'Assurance, verification, and quality of agent outputs.' },
  { icon: 'bi-diagram-3', title: 'Multi-Agent Coordination', desc: 'Communication and orchestration across multiple AI agents.' },
  { icon: 'bi-tools', title: 'Agent Tool Use & Environments', desc: 'How agents interact with IDEs, CI/CD, and development tools.' },
  { icon: 'bi-exclamation-triangle', title: 'Failure Modes & Root Causes', desc: 'Understanding when and why AI agents fail.' },
  { icon: 'bi-lock', title: 'Safety, Ethics & Compliance', desc: 'Responsible deployment and governance of AI Teammates.' },
  { icon: 'bi-graph-up-arrow', title: 'Adoption & Productivity', desc: 'Measuring the impact of AI agents on developer workflows.' },
  { icon: 'bi-cash-stack', title: 'Economic Cost & Impact', desc: 'Cost-benefit analysis of agentic software development.' },
  { icon: 'bi-gear-wide-connected', title: 'AgentOps & Release Engineering', desc: 'Operationalizing and deploying AI agent systems.' },
]

const dates = [
  { date: '<s>May 10th</s> <s>June 1st, 2026</s>', event: '<s>Workshop Paper Submission</s>' },
  { date: '<s>June 10th, 2026</s>', event: '<s>Workshop Paper Notification</s>' },
  { date: 'August 9th, 2026', event: 'Workshop @ KDD 2026, Jeju, Korea' },
]

const organizers = [
  {
    name: 'Hao Li',
    website: 'https://leo-lihao.github.io',
    photo: '/images/haoli.jpg',
    affiliation: 'Queen\'s University',
    role: '',
    bio: 'Postdoctoral Fellow at Queen\'s University. Co-Chair of the MSR 2026 Mining Challenge. Research focuses on agentic workflows at the intersection of Software Engineering and AI. Serves on PCs for ICSE, FSE, ASE, ICSME, SANER, and AIware.',
  },
  {
    name: 'Haoxiang Zhang',
    website: 'https://haoxianghz.gitlab.io/homepage/',
    photo: '/images/haoxiangzhang.jpg',
    objectPosition: 'center 15%',
    affiliation: 'Queen\'s University',
    role: '',
    bio: 'Adjunct Research Associate at Queen\'s University. Specializes in mining large-scale software repositories and building AI-driven analytics. Co-Chair of the MSR 2026 Mining Challenge.',
  },
  {
    name: 'Jie M. Zhang',
    website: 'https://sites.google.com/view/jie-zhang',
    photo: '/images/jiezhang.jpg',
    objectPosition: 'center 15%',
    affiliation: 'King\'s College London',
    role: '',
    bio: 'Lecturer at King\'s College London and visiting AI scientist at Mistral AI. Winner of the 2025 ACM SIGSOFT Early Research Award. PC Co-Chair for ASE 2027 and ISSRE 2026. Named one of the Top 15 Global Chinese Female Young Scholars in Interdisciplinary AI (2023).',
  },
  {
    name: 'Yiling Lou',
    website: 'https://yilinglou.github.io/',
    photo: '/images/yilinglou.jpg',
    affiliation: 'UIUC',
    role: '',
    bio: 'Assistant Professor in the Siebel School of Computing and Data Science at UIUC. Research on LLMs for code, software agents, and trustworthiness. Program Co-Chair for LLM4Code (2024–2026) and AIware (2025). Multiple ACM SIGSOFT and IEEE TCSE Distinguished Paper Awards.',
  },
  {
    name: 'Baishakhi Ray',
    website: 'https://rayb.info/',
    photo: '/images/baishakhiray.jpg',
    affiliation: 'Columbia University',
    role: '',
    bio: 'Associate Professor at Columbia University working at the intersection of AI, Software Engineering, and Security. Recipient of IEEE TCSE Rising Star, NSF CAREER, IBM Faculty, and VMWare Faculty awards. Amazon Visiting Academic.',
  },
  {
    name: 'Thomas Zimmermann',
    website: 'https://thomas-zimmermann.com/',
    photo: '/images/thomaszimmermann.jpg',
    affiliation: 'UC Irvine',
    role: '',
    bio: 'Chancellor\'s Professor and Bren Chair at UC Irvine. 20+ years of experience, 100+ publications cited 30,000+ times. Fellow of ACM, IEEE, and AAAS. Recipient of the IEEE TCSE Edward J. McCluskey Technical Achievement Award.',
  },
  {
    name: 'Ahmed E. Hassan',
    website: 'https://research.cs.queensu.ca/home/ahmed/home/',
    photo: '/images/ahmedhassan.jpg',
    affiliation: 'Queen\'s University',
    role: '',
    bio: 'Mustafa Prize Laureate. Fellow of ACM, IEEE, and AAIA. Canada Research Chair in Software Engineering. Founder of the AI-Augmented SE, MSR, and AIware communities. Member of the Royal Society of Canada. The only person to receive both the ACM SIGSOFT Influential Educator Award (2019) and the IEEE TCSE Distinguished Educator Award (2020).',
  },
]

// Author affiliations verified against the camera-ready OpenReview PDFs (pdfs/).
const acceptedPapersData = [
  {
    title: 'Aleena: Alignment Agent for Research Software Engineering Collaborations',
    authors: [
      { name: 'Kshitij Dani', orgs: ['University of Washington'] },
      { name: 'Cordero Core', orgs: ['University of Washington'] },
      { name: 'Landung Setiawan', orgs: ['University of Washington'] },
      { name: 'Carlos Garcia Jurado Suarez', orgs: ['University of Washington'] },
      { name: 'Anshul Tambay', orgs: ['University of Washington'] },
      { name: 'Vani Mandava', orgs: ['University of Washington'] },
      { name: 'Anant Mittal', orgs: ['University of Washington'] },
    ],
    id: 'W806qV5FMl',
  },
  {
    title: 'Beyond Simpson’s Paradox: A Cascade of Confounders in AI Agent Pull-Request Co-Authorship',
    authors: [
      { name: 'Haoran Yu', orgs: ['Independent Researcher'] },
      { name: 'Xiaochong Jiang', orgs: ['Independent Researcher'] },
      { name: 'Lifei Liu', orgs: ['Independent Researcher'] },
      { name: 'Su Wang', orgs: ['Carnegie Mellon University'] },
      { name: 'Pin Qian', orgs: ['Carnegie Mellon University'] },
      { name: 'Yihang Chen', orgs: ['Georgia Institute of Technology'] },
    ],
    id: 'Vry8s4qhsG',
  },
  {
    title: 'CAGE for Agentic Software Engineering: Field-Level Authorization at the AI Teammate Tool Boundary',
    authors: [
      { name: 'José Luis Delgado', orgs: ['Universitat Oberta de Catalunya'] },
    ],
    id: 'qMWWIpAu1r',
  },
  {
    title: 'Cost-Efficient Agentic Repository Setup for Automated SWE-bench Instance Construction',
    authors: [
      { name: 'Jaehyeon Shin', orgs: ['Kyung Hee University'] },
    ],
    id: 'BuOCdojk2w',
  },
  {
    title: 'Cross-Model LLM Code Review: Should you use Claude to review Codex or vice versa?',
    authors: [
      { name: 'Zuodong Xiang', orgs: ['University of California, Davis'] },
      { name: 'Yike Zhang', orgs: ['Johns Hopkins University'] },
      { name: 'Hailu Xu', orgs: ['California State University, Long Beach'] },
      { name: 'YueMing Zhang', orgs: ['California State University, Long Beach'] },
    ],
    id: 'Fh32FqhIfG',
  },
  {
    title: 'Early Adoption of Agentic Coding Tools by GitHub Projects',
    authors: [
      { name: 'Maliha Noushin Raida', orgs: ['Rochester Institute of Technology'] },
      { name: 'Daqing Hou', orgs: ['Rochester Institute of Technology'] },
    ],
    id: 'FIZmOuvISe',
  },
  {
    title: 'Habituation at the Gate: Rising Approval and Declining Scrutiny in Human Review of AI Agent Code',
    authors: [
      { name: 'Haoran Yu', orgs: ['Independent Researcher'] },
      { name: 'Lifei Liu', orgs: ['Independent Researcher'] },
      { name: 'Xiaochong Jiang', orgs: ['Independent Researcher'] },
      { name: 'Yuwen Jia', orgs: ['Independent Researcher'] },
      { name: 'Su Wang', orgs: ['Carnegie Mellon University'] },
      { name: 'Pin Qian', orgs: ['Carnegie Mellon University'] },
      { name: 'Yihang Chen', orgs: ['Georgia Institute of Technology'] },
    ],
    id: '2ZvHEHw4IZ',
  },
  {
    title: 'LLM-as-Code: Agentic Programming for Agent Harness',
    authors: [
      { name: 'Junjia Qi', orgs: ['City University of Hong Kong'] },
      { name: 'Zichuan Fu', orgs: ['City University of Hong Kong'] },
      { name: 'Jingtong Gao', orgs: ['City University of Hong Kong'] },
      { name: 'Wenlin Zhang', orgs: ['City University of Hong Kong'] },
      { name: 'Hanyu Yan', orgs: ['City University of Hong Kong'] },
      { name: 'Xian Wu', orgs: ['Tencent'] },
      { name: 'Xiangyu Zhao', orgs: ['City University of Hong Kong'] },
    ],
    id: 'AXRMBC8j7c',
  },
  {
    title: 'Memory as a Service (MaaS): Purpose-Bound Memory Mediation for Cooperative Agents',
    authors: [
      { name: 'Haichang Li', orgs: ['George Mason University'] },
    ],
    id: 'hWEO8cKDvS',
  },
  {
    title: 'Position: Coding Benchmarks Are Misaligned with Agentic Software Engineering',
    authors: [
      { name: 'Maria I. Gorinova', orgs: ['Tessl'] },
      { name: 'Macey Baker', orgs: ['Tessl'] },
      { name: 'Amy Heineike', orgs: ['Tessl'] },
      { name: 'Maksim Shaposhnikov', orgs: ['Tessl'] },
      { name: 'Rob Willoughby', orgs: ['Tessl'] },
      { name: 'Dru Knox', orgs: ['Tessl'] },
    ],
    id: 't32Hgnk7ou',
  },
  {
    title: 'RANGER: Repository-level Agent for Graph-Enhanced Retrieval',
    authors: [
      { name: 'Pratik Shah', orgs: ['Georgia Institute of Technology'] },
      { name: 'Rajat Ghosh', orgs: ['Nutanix'] },
      { name: 'Aryan Singhal', orgs: ['Nutanix'] },
      { name: 'Debojyoti Dutta', orgs: ['Nutanix'] },
    ],
    id: 'WBCZRyP0HB',
  },
  {
    title: 'Rethinking Failure Attribution in Multi-Agent Systems: A Multi-Perspective Benchmark and Evaluation',
    authors: [
      { name: 'Yeonjun In', orgs: ['KAIST'] },
      { name: 'Mehrab Tanjim', orgs: ['Adobe Research'] },
      { name: 'Jayakumar Subramanian', orgs: ['Adobe Research'] },
      { name: 'Sungchul Kim', orgs: ['Adobe Research'] },
      { name: 'Uttaran Bhattacharya', orgs: ['Adobe Research'] },
      { name: 'Wonjoong Kim', orgs: ['KAIST'] },
      { name: 'Sangwu Park', orgs: ['KAIST'] },
      { name: 'Somdeb Sarkhel', orgs: ['Adobe Research'] },
      { name: 'Chanyoung Park', orgs: ['KAIST'] },
    ],
    type: 'Oral',
    id: 'oBaY1EFz6z',
  },
  {
    title: 'Self-EvolveRec: Self-Evolving Recommender Systems with LLM-based Directional Feedback',
    authors: [
      { name: 'Sein Kim', orgs: ['KAIST'] },
      { name: 'Sangwu Park', orgs: ['KAIST'] },
      { name: 'HongSeok Kang', orgs: ['KAIST'] },
      { name: 'Wonjoong Kim', orgs: ['KAIST'] },
      { name: 'Jimin Seo', orgs: ['KAIST'] },
      { name: 'Yeonjun In', orgs: ['KAIST'] },
      { name: 'Kanghoon Yoon', orgs: ['KAIST'] },
      { name: 'Hyunsik Jeon', orgs: ['Microsoft'] },
      { name: 'Chanyoung Park', orgs: ['KAIST'] },
    ],
    type: 'Oral',
    id: 'fBLhmZ1aNN',
  },
  {
    title: 'Skill-as-API: Confidential Multi-Agent Coordination for Agentic Software Engineering',
    authors: [
      { name: 'Ziwei Zhao', orgs: ['Technical University of Munich'] },
      { name: 'Yu Gu', orgs: ['Technical University of Munich'] },
      { name: 'Haojun Liang', orgs: ['Technical University of Munich'] },
      { name: 'Chen Zhang', orgs: ['Technical University of Munich'] },
      { name: 'Xizhi Ding', orgs: ['London Business School'] },
    ],
    id: 'mBwHulqlqO',
  },
  {
    title: 'SVRepair: Structured Visual Reasoning for Automated Program Repair',
    authors: [
      { name: 'Jincheng Wang', orgs: ['Ant Group'] },
      { name: 'Liwei Luo', orgs: ['Ant Group'] },
      { name: 'Xiaoxuan Tang', orgs: [] },
      { name: 'Jingxuan Xu', orgs: ['Ant Group'] },
      { name: 'Sheng Zhou', orgs: ['Zhejiang University'] },
      { name: 'Dajun Chen', orgs: ['Ant Group'] },
      { name: 'Wei Jiang', orgs: ['Ant Group'] },
      { name: 'Yong Li', orgs: ['Ant Group'] },
    ],
    type: 'Oral',
    id: '7vFgF9DV1G',
  },
  {
    title: 'When Does Restricting a Coding Agent to `execute_code` Help? A Regime × Agent-Design Ablation',
    authors: [
      { name: 'Hong Yang', orgs: ['Rochester Institute of Technology'] },
      { name: 'Qi Yu', orgs: ['Rochester Institute of Technology'] },
      { name: 'Travis Desell', orgs: ['Rochester Institute of Technology'] },
    ],
    id: 'kbkm1Y4BtY',
  },
  {
    title: 'Tracing the Messy Middle: A Runtime Transaction Schema for AI Coding Agents',
    authors: [
      { name: 'Yuelin Hou', orgs: ['QingCheng.AI', 'Duke University'] },
      { name: 'Haorui Li', orgs: ['QingCheng.AI'] },
      { name: 'Lu Chen', orgs: ['QingCheng.AI', 'Fudan University'] },
      { name: 'Zhen Chen', orgs: ['QingCheng.AI', 'University of Science and Technology of China'] },
    ],
    id: 'GwP03jl6Ux',
  },
  {
    title: 'Trust but Verify? Uncovering the Security Debt of Autonomous Coding Agents',
    authors: [
      { name: 'A H M Nazmus Sakib', orgs: ['University of Texas at San Antonio'] },
      { name: 'Dipayan Banik', orgs: ['Danovo Energy Solutions'] },
      { name: 'Murtuza Jadliwala', orgs: ['University of Texas at San Antonio'] },
    ],
    id: 'K9Wv2pVlIZ',
  },
  {
    title: 'Understanding Human Review Behavior in Agent-Authored Pull Requests: Evidence from AIDev',
    authors: [
      { name: 'Chen Zhang', orgs: ['Nanjing University'] },
      { name: 'Junxi Wang', orgs: ['Hefei University of Technology'] },
      { name: 'Shihong Huang', orgs: ['Nanjing Agricultural University'] },
    ],
    id: '57OKmT7Wlz',
  },
  {
    title: 'Task-Specialized Harness Refinement Across Financial Decision Tasks',
    authors: [
      { name: 'Jinuk Lee', orgs: ['Kyungpook National University'] },
    ],
    id: '9LHVuRrbYx',
  },
]

// One org shared by all authors → plain org line; otherwise superscript markers
// per author, with a numbered org legend below.
function formatPaperAuthors(authors) {
  const orgs = []
  for (const a of authors) {
    for (const o of a.orgs) if (!orgs.includes(o)) orgs.push(o)
  }
  const multi = orgs.length > 1
  const authorsHtml = authors
    .map((a) => {
      const marks = a.orgs.map((o) => orgs.indexOf(o) + 1)
      return a.name + (multi && marks.length ? `<sup>${marks.join(',')}</sup>` : '')
    })
    .join(', ')
  const affilHtml = multi
    ? orgs.map((o, i) => `<sup>${i + 1}</sup>${o}`).join(' &nbsp;·&nbsp; ')
    : orgs[0] || ''
  return { authorsHtml, affilHtml }
}

const acceptedPapers = acceptedPapersData
  .map((p) => ({ ...p, ...formatPaperAuthors(p.authors) }))
  .sort((a, b) => a.title.localeCompare(b.title))

const oralPapers = acceptedPapers.filter((paper) => paper.type === 'Oral')

const keynoteSpeakers = [
  {
    id: 'speaker-behrooz',
    name: 'Behrooz Omidvar-Tehrani',
    role: 'Science Lead',
    organization: 'AWS Agentic AI',
    website: 'https://www.linkedin.com/in/behroozomidvar',
    photo: '/images/behrooz.png',
    time: '10:10 – 11:00',
    title: 'Teaching Coding Agents to Remember What Matters',
    bio: 'Behrooz Omidvar-Tehrani is a Senior Applied Scientist and Science Lead at AWS Agentic AI, where he drives research on agentic code transformation and coding agents. Previously, he was a Research Scientist at LIG (Grenoble Informatics Laboratory) and NAVER LABS Europe, and a Postdoctoral Researcher at The Ohio State University. He holds a PhD in Mathematics and Computer Science from Université Grenoble Alpes.',
    abstract: 'Large language models are stateless, retaining nothing between invocations. Yet developers increasingly expect AI coding agents to behave like teammates who remember project conventions, adapt to personal style, recall past failures, and coordinate across a team. The challenge is not just remembering, but also learning what matters and forgetting what does not. This talk examines what a principled memory system for coding agents entails. We discuss how distinct forms of remembering, from recalling past interactions to internalizing codebase structure, contribute to the developer experience in fundamentally different ways. We then expand the scope beyond the individual, exploring how memory extends to the team level, where shared conventions and collective knowledge must be captured and respected. Drawing on concrete examples, we identify where current designs succeed and where critical gaps persist, particularly around lifecycle governance, retrieval and ranking, long-term adaptation, and multi-player coordination.',
  },
]

const pc = [
  { name: 'Sasha Aptlin', affiliation: 'ReachRx' },
  { name: 'Shraddha Barke', affiliation: 'Microsoft' },
  { name: 'Sandesh Basrur', affiliation: 'Judson College' },
  { name: 'Arjun Chakraborty', affiliation: 'Microsoft' },
  { name: 'Yihang Chen', affiliation: 'Georgia Institute of Technology' },
  { name: 'Gabriele De Vito', affiliation: 'University of Salerno' },
  { name: 'Kadir Alpaslan Demir', affiliation: 'Texas A&M University' },
  { name: 'Abhishek Dharmaratnakar', affiliation: 'Google' },
  { name: 'Karim Elish', affiliation: 'Florida Polytechnic University' },
  { name: 'Quanzhi Fu', affiliation: 'Virginia Tech' },
  { name: 'Suwen Ge', affiliation: 'Facebook' },
  { name: 'Advitya Gemawat', affiliation: 'Microsoft' },
  { name: 'Sayan Ghosh', affiliation: 'Amazon' },
  { name: 'Md. Asif Haider', affiliation: 'University of California, Irvine' },
  { name: 'Daqing Hou', affiliation: 'Clarkson University' },
  { name: 'Anisha Anil Jadhav', affiliation: 'Independent' },
  { name: 'Vineeth Kalluru', affiliation: 'NVIDIA' },
  { name: 'Daming Li', affiliation: 'LinkedIn' },
  { name: 'Jianan Liu', affiliation: 'Meta Platforms, Inc' },
  { name: 'Jiao Liu', affiliation: 'Morgan Stanley' },
  { name: 'Akshay Mallipeddi', affiliation: 'Splunk Inc' },
  { name: 'Rui Melo', affiliation: 'Carnegie Mellon University' },
  { name: 'Siba Mishra', affiliation: 'C. V. Raman Global University' },
  { name: 'Bhoomi Kalpesh Patel', affiliation: 'Facebook' },
  { name: 'Prajwal Sanjay Pisal', affiliation: 'Independent' },
  { name: 'Ninaad Rao', affiliation: 'Cisco' },
  { name: 'Yu Shi', affiliation: 'Queen\'s University' },
  { name: 'Siddharth Vohra', affiliation: 'Carnegie Mellon University' },
  { name: 'Haoran Yu', affiliation: 'University of Florida' },
  { name: 'Zhimin Zhao', affiliation: "Queen's University", role: 'Web Chair' }
]


function scrollTo(id) {
  navCollapsed.value = true
  const el = document.getElementById(id)
  if (el) el.scrollIntoView({ behavior: 'smooth' })
}

function handleScroll() {
  const scrollY = window.scrollY + 100
  for (let i = sections.length - 1; i >= 0; i--) {
    const el = document.getElementById(sections[i].id)
    if (el && el.offsetTop <= scrollY) {
      activeSection.value = sections[i].id
      break
    }
  }
}

onMounted(() => window.addEventListener('scroll', handleScroll))
onUnmounted(() => window.removeEventListener('scroll', handleScroll))
</script>

<template>
  <!-- Navbar -->
  <nav class="navbar navbar-expand-lg navbar-dark fixed-top">
    <div class="container">
      <a class="navbar-brand fw-bold" href="#" @click.prevent="scrollTo('home')">
        <i class="bi bi-robot me-2"></i>Agentic SE @ KDD'26
      </a>
      <button
        class="navbar-toggler border-0"
        type="button"
        @click="navCollapsed = !navCollapsed"
        aria-label="Toggle navigation"
      >
        <span class="navbar-toggler-icon"></span>
      </button>
      <div class="collapse navbar-collapse" :class="{ show: !navCollapsed }">
        <ul class="navbar-nav ms-auto">
          <li class="nav-item" v-for="s in sections" :key="s.id">
            <a
              class="nav-link"
              :class="{ active: activeSection === s.id }"
              href="#"
              @click.prevent="scrollTo(s.id)"
            >{{ s.label }}</a>
          </li>
        </ul>
      </div>
    </div>
  </nav>

  <!-- Hero Text -->
  <section id="home" class="hero-text-section pt-5 mt-5 pb-4">
    <div class="container position-relative" style="z-index: 1;">
      <div class="row">
        <div class="col-lg-10">
          <span class="hero-badge">KDD 2026 Workshop</span>
          <h1 class="hero-title">
            Agentic Software Engineering
            <span style="color: var(--accent-dark);">(SE 3.0)</span>
          </h1>
          <p class="hero-subtitle">The Rise of AI Teammates</p>
          <p class="hero-meta mb-2">
            <i class="bi bi-calendar-event"></i> August 9th, 2026
            <span class="mx-2">|</span>
            <i class="bi bi-geo-alt"></i> Jeju ICC, Korea
            <span class="mx-2">|</span>
            <i class="bi bi-building"></i> Room: Samda B
            <span class="mx-2">|</span>
            <i class="bi bi-clock"></i> Half-Day (Morning)
          </p>
          <p class="hero-meta" style="max-width: 800px; margin-top: 1.5rem; font-size: 1.05rem; line-height: 1.7;">
            Co-located with the <a href="https://kdd2026.kdd.org/" target="_blank" rel="noopener noreferrer" style="color: inherit; text-decoration: underline;">ACM SIGKDD 2026</a>.
            Anchored by the <a href="https://huggingface.co/datasets/hao-li/AIDev" target="_blank" rel="noopener noreferrer" style="color: var(--accent-dark); font-weight: bold; text-decoration: none;">AIDev dataset</a> — over one million agentic pull requests
            from Claude Code, OpenAI Codex, and GitHub Copilot.
          </p>
        </div>
      </div>
    </div>
  </section>

  <!-- Program Sketch -->
  <section id="program" class="section section-alt">
    <div class="container">
      <h2 class="section-title">Program Schedule</h2>
      <hr class="section-divider">
      <p class="mb-4 text-muted">Half-day (morning) workshop, August 9th, 2026, in Room Samda B at the Jeju ICC.</p>
      <div class="col-lg-10">
        <table class="table program-table">
          <thead>
            <tr>
              <th style="width: 160px;">Time</th>
              <th>Event</th>
            </tr>
          </thead>
          <tbody>
            <tr><td>8:00 – 8:10</td><td>Opening Remarks</td></tr>
            <tr>
              <td>8:10 – 9:00</td>
              <td>
                Paper Oral Presentations
                <div class="schedule-oral-list">
                  <div v-for="paper in oralPapers" :key="paper.id" class="schedule-oral-item">
                    <a class="schedule-oral-title" :href="`https://openreview.net/pdf?id=${paper.id}`" target="_blank" rel="noopener noreferrer">{{ paper.title }}</a>
                    <span class="schedule-oral-authors" v-html="paper.authorsHtml"></span>
                    <span v-if="paper.affilHtml" class="schedule-oral-affiliations" v-html="paper.affilHtml"></span>
                  </div>
                </div>
              </td>
            </tr>
            <tr><td>9:00 – 10:10</td><td><a href="#" @click.prevent="scrollTo('accepted-papers')" class="schedule-link">Poster Session</a> (ICC 1 Foyer, between Halla and Samda Hall) + Coffee Break (coffee served 9:30 – 10:00)</td></tr>
            <tr><td>10:10 – 11:00</td><td>Keynote: <em><a href="#" @click.prevent="scrollTo('keynote')" class="schedule-link">{{ keynoteSpeakers[0].title }}</a></em> — {{ keynoteSpeakers[0].name }} <span class="schedule-keynote-org">({{ keynoteSpeakers[0].organization }})</span></td></tr>
            <tr><td>11:00 – 12:00</td><td>Discussion Session: Mining the AI Teammate – Data, Discovery, and the Future of Agentic Software Engineering</td></tr>
          </tbody>
        </table>
      </div>
    </div>
  </section>

  <!-- Keynote Speakers -->
  <section id="keynote" class="section">
    <div class="container">
      <h2 class="section-title">Keynote Speaker</h2>
      <hr class="section-divider">
      <div v-for="speaker in keynoteSpeakers" :key="speaker.id" class="keynote-speaker-card mb-5">
        <div class="row align-items-start">
          <div class="col-md-3 text-center mb-3 mb-md-0">
            <a :href="speaker.website" target="_blank" rel="noopener noreferrer">
              <img class="keynote-avatar" :src="speaker.photo" :alt="speaker.name">
            </a>
            <h5 class="mt-3 mb-1">
              <a :href="speaker.website" target="_blank" rel="noopener noreferrer" style="color: inherit; text-decoration: underline; text-underline-offset: 3px;">
                {{ speaker.name }}
              </a>
            </h5>
            <p class="affiliation mb-0">{{ speaker.role }}</p>
            <p class="speaker-organization mb-0">{{ speaker.organization }}</p>
          </div>
          <div class="col-md-9">
            <h4 class="fw-bold mb-2" style="color: var(--accent-dark);">{{ speaker.title }}</h4>
            <p class="text-muted mb-2" style="font-size: 0.9rem;">
              <i class="bi bi-clock me-1"></i>{{ speaker.time }}
            </p>
            <p style="font-size: 0.95rem; line-height: 1.8; opacity: 0.85;" class="mb-3">{{ speaker.abstract }}</p>
            <p style="font-size: 0.9rem; line-height: 1.7; opacity: 0.7;"><strong>Bio:</strong> {{ speaker.bio }}</p>
          </div>
        </div>
      </div>
    </div>
  </section>

  <!-- Accepted Papers -->
  <section id="accepted-papers" class="section section-alt">
    <div class="container">
      <h2 class="section-title">Accepted Papers</h2>
      <hr class="section-divider">
      <div class="col-lg-10">
        <p class="mb-4 text-muted">Posters will be displayed in the ICC 1 Foyer, between Halla and Samda Hall.</p>
        <div class="accepted-papers-list">
          <article class="accepted-paper-item" v-for="paper in acceptedPapers" :key="paper.title">
            <div class="accepted-paper-heading">
              <h3 class="accepted-paper-title">
                <a v-if="paper.id" class="accepted-paper-title-link" :href="`https://openreview.net/pdf?id=${paper.id}`" target="_blank" rel="noopener noreferrer">{{ paper.title }}</a>
                <template v-else>{{ paper.title }}</template>
              </h3>
              <span v-if="paper.type === 'Oral'" class="accepted-paper-tag">Oral</span>
            </div>
            <p class="accepted-paper-authors" v-html="paper.authorsHtml"></p>
            <p v-if="paper.affilHtml" class="accepted-paper-affiliations" v-html="paper.affilHtml"></p>
          </article>
        </div>
      </div>
    </div>
  </section>

  <!-- Overview -->
  <section class="section">
    <div class="container">
      <h2 class="section-title">Workshop Overview</h2>
      <hr class="section-divider">
      <div class="row">
        <div class="col-lg-10">
          <p class="lead" style="font-size: 1.05rem;">
            We are witnessing the emergence of <strong>Agentic Software Engineering (SE 3.0)</strong>,
            where AI agents act as autonomous AI Teammates performing complex tasks such as coding,
            debugging, and testing. As AI Teammates generate a vast new category of digital artifacts,
            they introduce unique opportunities and challenges related to human-AI collaboration,
            trustworthiness, and economic impact.
          </p>
          <p style="font-size: 1.02rem; color: var(--text-muted);">
            This workshop serves as the premier forum for addressing these challenges, anchored by the
            launch of the <a href="https://huggingface.co/datasets/hao-li/AIDev" target="_blank" rel="noopener noreferrer" style="font-weight: bold; color: inherit; text-decoration: underline;">AIDev dataset</a>. Comprising over one million agentic pull requests generated
            by AI Teammates such as Claude Code, OpenAI Codex, and GitHub Copilot, AIDev provides the
            empirical evidence needed to understand the behaviors of AI Teammates. This workshop features
            insights from major industry players and academic pioneers, and aims to define a roadmap for
            a world where AI Teammates and human developers build the future together.
          </p>
        </div>
      </div>
    </div>
  </section>

  <!-- Submission / CTA -->
  <section id="submission" class="section section-alt">
    <div class="container">
      <h2 class="section-title">Call for Papers</h2>
      <hr class="section-divider">
      <div>
        <div class="row align-items-center">
          <div class="col-lg-10">
            <p style="opacity: 0.9; font-size: 0.98rem; line-height: 1.8;">
              We invite submissions of original research papers, position papers, and demo papers
              on the following topics of interest. Papers should follow the ACM KDD format.
              All accepted papers will be presented at the workshop.
            </p>
            <h5 class="fw-bold mt-4 mb-2">Topics of Interest:</h5>
            <ul style="opacity: 0.85; font-size: 0.92rem;" class="mb-4">
              <li v-for="t in topics" :key="t.title" class="mb-1">
                <strong>{{ t.title }}</strong>: {{ t.desc }}
              </li>
            </ul>
            <h5 class="fw-bold mt-4 mb-2">Note on Data:</h5>
            <p style="opacity: 0.85; font-size: 0.92rem;" class="mb-4">
              While we encourage studies leveraging AIDev, submissions are not limited to this
              dataset. We welcome research proposing original datasets, as well as data mining
              and tooling studies.
            </p>
            <h5 class="fw-bold mt-4 mb-2">Submission Guidelines:</h5>
            <ul style="opacity: 0.85; font-size: 0.92rem;" class="mb-4">
              <li>Long papers: up to 8 pages (excluding references)</li>
              <li>Short papers: up to 4 pages (excluding references)</li>
              <li>Position papers: up to 4 pages (excluding references)</li>
              <li>Format: <a href="https://kdd2026.kdd.org/research-track-call-for-papers/" target="_blank" rel="noopener noreferrer" style="color: inherit; text-decoration: underline;">ACM double-column (KDD 2026 template)</a>. Submissions must use <code>\documentclass[sigconf,anonymous,review]{acmart}</code>.</li>
              <li>Review: All submissions are double-blind. Please anonymize your submission.</li>
              <li>Proceedings: The workshop is a non-archival, no formal proceedings.</li>
            </ul>
            <a href="https://openreview.net/group?id=KDD.org/2026/Workshop/AgenticSE" target="_blank" rel="noopener" class="btn-submit mt-2">
              <i class="bi bi-box-arrow-up-right"></i> Submit Paper
            </a>
          </div>
        </div>
      </div>
    </div>
  </section>

  <!-- Important Dates -->
  <section id="dates" class="section">
    <div class="container">
      <h2 class="section-title">Important Dates</h2>
      <hr class="section-divider">
      <div class="col-lg-8">
        <div class="date-item" v-for="d in dates" :key="d.date">
          <span class="date-badge" v-html="d.date"></span>
          <span style="font-size: 1.02rem;" v-html="d.event"></span>
        </div>
        <p class="text-muted mt-3" style="font-size: 0.88rem;">
          <i class="bi bi-info-circle me-1"></i>
          All deadlines are at 11:59 PM AoE (Anywhere on Earth).
        </p>
      </div>
    </div>
  </section>

  <!-- Reviewer Self-Nomination -->
  <section id="reviewer" class="section section-alt">
    <div class="container">
      <h2 class="section-title">Reviewer Self-Nomination</h2>
      <hr class="section-divider">
      <div class="col-lg-10">
        <p style="font-size: 1.02rem; line-height: 1.8;">
          We are looking for qualified reviewers with expertise in AI agents, software engineering,
          and related areas. If you are interested in serving as a reviewer for the
          Agentic SE @ KDD'26 workshop, please fill out the self-nomination form below.
        </p>
        <a href="https://docs.google.com/forms/d/e/1FAIpQLSeCNMUCDfMx9P9uxhqARNSdKuzQlpdOyeQu14NpWk2AaF2-Gg/viewform" target="_blank" rel="noopener noreferrer" class="btn-submit mt-2">
          <i class="bi bi-pencil-square"></i> Self-Nominate as Reviewer
        </a>
      </div>
    </div>
  </section>

  <!-- Organizers -->
  <section id="organizers" class="section">
    <div class="container">
      <h2 class="section-title">Organizing Committee</h2>
      <hr class="section-divider">
      <div class="row g-4 justify-content-center">
        <div class="col-6 col-md-4 col-lg-3" v-for="o in organizers" :key="o.name">
          <div class="organizer-card">
            <a v-if="o.website" :href="o.website" target="_blank" rel="noopener noreferrer">
              <img class="avatar" :src="o.photo" :alt="o.name" :style="o.objectPosition ? { objectPosition: o.objectPosition } : {}">
            </a>
            <img v-else class="avatar" :src="o.photo" :alt="o.name" :style="o.objectPosition ? { objectPosition: o.objectPosition } : {}">
            <h5>
              <a v-if="o.website" :href="o.website" target="_blank" rel="noopener noreferrer" style="color: inherit; text-decoration: underline; text-underline-offset: 3px;">
                {{ o.name }}
              </a>
              <template v-else>{{ o.name }}</template>
            </h5>
            <p class="affiliation">{{ o.affiliation }}</p>
            <span v-if="o.role" class="role-badge">{{ o.role }}</span>
          </div>
        </div>
      </div>

    </div>
  </section>

  <!-- Program Committee -->
  <section id="committee" class="section section-alt">
    <div class="container">
      <h2 class="section-title">Program Committee</h2>
      <hr class="section-divider">
      <div class="col-lg-10">
        <ul class="pc-list">
          <li v-for="p in pc" :key="p.name" class="pc-item">
            <div class="pc-name">
              <i class="bi bi-person-badge"></i>{{ p.name }}<span v-if="p.role" class="pc-role"> ({{ p.role }})</span>
            </div>
            <div v-if="p.affiliation" class="pc-affiliation">{{ p.affiliation }}</div>
          </li>
        </ul>
      </div>
    </div>
  </section>

  <!-- Footer -->
  <footer class="site-footer">
    <div class="container text-center">
      <p class="mb-1">
        <strong>Agentic Software Engineering (SE 3.0)</strong> — KDD 2026 Workshop
      </p>
      <p class="mb-1">
        <i class="bi bi-geo-alt me-1"></i> Jeju, Korea &middot;
        <i class="bi bi-calendar3 ms-2 me-1"></i> August 9th, 2026
      </p>
      <p class="mb-3 mt-3" style="font-size: 0.9rem;">
        <strong>Contact us:</strong> Please send questions and enquiries to <a href="mailto:hao.li@queensu.ca" style="color: inherit; text-decoration: underline;">hao.li@queensu.ca</a>
      </p>
      <p class="mb-0" style="font-size: 0.82rem; opacity: 0.6;">
        &copy; 2026 Agentic SE Workshop. Co-located with <a href="https://kdd2026.kdd.org/" target="_blank" rel="noopener noreferrer" style="color: inherit; text-decoration: underline;">ACM SIGKDD 2026</a>.
      </p>
    </div>
  </footer>
</template>
