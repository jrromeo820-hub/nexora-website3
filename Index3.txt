// Year in footer
document.getElementById("year").textContent = new Date().getFullYear();

// Mobile nav
const navToggle = document.getElementById("navToggle");
const navMenu = document.getElementById("navMenu");
const navbar = document.getElementById("navbar");

function setMenu(open) {
  navMenu.classList.toggle("open", open);
  navToggle.setAttribute("aria-expanded", String(open));
}

navToggle?.addEventListener("click", () => {
  const isOpen = navMenu.classList.contains("open");
  setMenu(!isOpen);
});

navMenu?.addEventListener("click", (e) => {
  if (e.target.matches("a")) setMenu(false);
});

// Navbar animation on scroll (shrink/blur)
window.addEventListener(
  "scroll",
  () => {
    const y = window.scrollY || 0;
    navbar.classList.toggle("scrolled", y > 16);
  },
  { passive: true }
);

// Reveal on scroll (fade-in)
const revealEls = document.querySelectorAll(".reveal");
const io = new IntersectionObserver(
  (entries) => {
    entries.forEach((entry) => {
      if (entry.isIntersecting) entry.target.classList.add("is-visible");
    });
  },
  { threshold: 0.12 }
);

revealEls.forEach((el) => io.observe(el));

// Active nav link highlighting
const sectionIds = ["services", "about", "portfolio", "testimonials", "contact"];
const navLinks = Array.from(document.querySelectorAll(".nav-link"));

const sectionEls = sectionIds
  .map((id) => document.getElementById(id))
  .filter(Boolean);

const activeIO = new IntersectionObserver(
  (entries) => {
    entries.forEach((entry) => {
      if (!entry.isIntersecting) return;
      const id = entry.target.getAttribute("id");
      navLinks.forEach((a) =>
        a.classList.toggle("active", a.getAttribute("href") === `#${id}`)
      );
    });
  },
  { rootMargin: "-40% 0px -55% 0px", threshold: 0.01 }
);

sectionEls.forEach((sec) => activeIO.observe(sec));

// Smooth scroll with offset (for sticky nav)
function smoothScrollTo(hash) {
  const el = document.querySelector(hash);
  if (!el) return;
  const navHeight = navbar?.offsetHeight || 0;
  const top = el.getBoundingClientRect().top + window.scrollY - navHeight + 2;
  window.scrollTo({ top, behavior: "smooth" });
}

document.querySelectorAll('a[href^="#"]').forEach((a) => {
  a.addEventListener("click", (e) => {
    const href = a.getAttribute("href");
    if (!href || href === "#") return;
    if (document.querySelector(href)) {
      e.preventDefault();
      smoothScrollTo(href);
      history.pushState(null, "", href);
    }
  });
});

// Contact form (client-side demo)
const form = document.getElementById("contactForm");
const note = document.getElementById("formNote");

form?.addEventListener("submit", (e) => {
  e.preventDefault();

  const data = new FormData(form);
  const name = (data.get("name") || "").toString().trim();
  const email = (data.get("email") || "").toString().trim();

  if (!name || !email) {
    note.textContent = "Please fill in the required fields.";
    return;
  }

  // Simulate sending
  note.textContent = "Sending…";
  setTimeout(() => {
    note.textContent =
      "Thanks — your message has been captured (demo). We’ll be in touch shortly.";
    form.reset();
  }, 700);
});
