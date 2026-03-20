<script setup>
import { ref, onMounted, onUnmounted } from 'vue'

const activeSection = ref('home')
const navCollapsed = ref(true)

const sections = [
  { id: 'home', label: 'Home' },
  { id: 'submission', label: 'Call for Papers' },
  { id: 'dates', label: 'Important Dates' },
  { id: 'program', label: 'Program' },
  { id: 'organizers', label: 'Organizers' },
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
  { date: 'TBD', event: 'Paper Submission Deadline' },
  { date: 'TBD', event: 'Notification of Acceptance' },
  { date: 'TBD', event: 'Workshop @ KDD 2026, Jeju, Korea' },
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
    website: 'https://haoxianghz.gitlab.io',
    photo: '/images/haoxiangzhang.jpg',
    affiliation: 'Queen\'s University',
    role: '',
    bio: 'Adjunct Research Associate at Queen\'s University. Specializes in mining large-scale software repositories and building AI-driven analytics. Co-Chair of the MSR 2026 Mining Challenge.',
  },
  {
    name: 'Jie M. Zhang',
    website: 'https://sites.google.com/view/jie-zhang',
    photo: '/images/jiezhang.jpg',
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

const expandedBios = ref({})

function toggleBio(name) {
  expandedBios.value[name] = !expandedBios.value[name]
}

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

  <!-- Hero -->
  <section id="home" class="hero-section" style="background-image: url('/images/background.jpg');">
    <div class="container position-relative" style="z-index: 1;">
      <div class="row">
        <div class="col-lg-9">
          <span class="hero-badge">KDD 2026 Workshop</span>
          <h1 class="hero-title">
            Agentic Software Engineering
            <span style="color: var(--accent);">(SE 3.0)</span>
          </h1>
          <p class="hero-subtitle">The Rise of AI Teammates</p>
          <p class="hero-meta mb-2">
            <i class="bi bi-calendar-event"></i> August TBD, 2026
            <span class="mx-2">|</span>
            <i class="bi bi-geo-alt"></i> Jeju, Korea
            <span class="mx-2">|</span>
            <i class="bi bi-clock"></i> Half-Day
          </p>
          <p class="hero-meta" style="max-width: 700px; margin-top: 1.5rem; font-size: 0.95rem; line-height: 1.7;">
            Co-located with the <a href="https://kdd2026.kdd.org/" target="_blank" rel="noopener noreferrer" style="color: inherit; text-decoration: underline;">ACM SIGKDD 2026</a>.
            Anchored by the <a href="https://huggingface.co/datasets/hao-li/AIDev" target="_blank" rel="noopener noreferrer" style="color: var(--accent); font-weight: bold; text-decoration: none;">AIDev dataset</a> — over one million agentic pull requests
            from Claude Code, OpenAI Codex, and GitHub Copilot.
          </p>
          <a href="https://kdd2026.kdd.org" target="_blank" rel="noopener" class="btn-register">
            <i class="bi bi-box-arrow-up-right"></i> Register at KDD 2026
          </a>
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
          <div class="col-lg-8">
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
            <h5 class="fw-bold mt-4 mb-2">Submission Guidelines:</h5>
            <ul style="opacity: 0.85; font-size: 0.92rem;" class="mb-3">
              <li>Long papers: up to 8 pages (excluding references)</li>
              <li>Short / position papers: up to 4 pages (excluding references)</li>
              <li>Format: <a href="https://kdd2026.kdd.org/research-track-call-for-papers/" target="_blank" rel="noopener noreferrer" style="color: inherit; text-decoration: underline;">ACM double-column (KDD 2026 template)</a></li>
            </ul>
          </div>
          <div class="col-lg-4 text-lg-end mt-3 mt-lg-0">
            <p class="mb-2" style="opacity: 0.7; font-size: 0.85rem;">Submission portal coming soon</p>
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
          <span class="date-badge">{{ d.date }}</span>
          <span style="font-size: 1.02rem;">{{ d.event }}</span>
        </div>
        <p class="text-muted mt-3" style="font-size: 0.88rem;">
          <i class="bi bi-info-circle me-1"></i>
          All deadlines are at 11:59 PM AoE (Anywhere on Earth).
        </p>
      </div>
    </div>
  </section>

  <!-- Program Sketch -->
  <section id="program" class="section section-alt">
    <div class="container">
      <h2 class="section-title">Program Sketch</h2>
      <hr class="section-divider">
      <p class="mb-4 text-muted">Half-day workshop. Detailed schedule to be announced.</p>
      <!-- <div class="col-lg-10">
        <table class="table program-table">
          <thead>
            <tr>
              <th style="width: 160px;">Time</th>
              <th>Event</th>
            </tr>
          </thead>
          <tbody>
            <tr><td>TBD</td><td>Opening Remarks</td></tr>
            <tr><td>TBD</td><td>Keynote Talk 1</td></tr>
            <tr><td>TBD</td><td>Paper Presentations — Session 1</td></tr>
            <tr><td>TBD</td><td>Coffee Break</td></tr>
            <tr><td>TBD</td><td>Keynote Talk 2</td></tr>
            <tr><td>TBD</td><td>Paper Presentations — Session 2</td></tr>
            <tr><td>TBD</td><td>Panel Discussion: The Future of AI Teammates</td></tr>
            <tr><td>TBD</td><td>Closing Remarks</td></tr>
          </tbody>
        </table>
      </div> -->
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
            <img class="avatar" :src="o.photo" :alt="o.name">
            <h5>
              <a v-if="o.website" :href="o.website" target="_blank" rel="noopener noreferrer" style="color: inherit; text-decoration: underline; text-underline-offset: 3px;">
                {{ o.name }}
              </a>
              <template v-else>{{ o.name }}</template>
            </h5>
            <p class="affiliation">{{ o.affiliation }}</p>
            <span v-if="o.role" class="role-badge">{{ o.role }}</span>
            <div v-if="expandedBios[o.name]" class="bio-text">{{ o.bio }}</div>
            <button class="bio-toggle" @click="toggleBio(o.name)">
              {{ expandedBios[o.name] ? 'Less' : 'Bio' }}
              <i :class="expandedBios[o.name] ? 'bi bi-chevron-up' : 'bi bi-chevron-down'" style="font-size: 0.7rem;"></i>
            </button>
          </div>
        </div>
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
        <i class="bi bi-calendar3 ms-2 me-1"></i> August TBD, 2026
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
