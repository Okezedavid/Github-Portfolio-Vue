<script setup>
  import PrevIcon from './icons/prevIcon.vue';
  import NextIcon from './icons/nextIcon.vue';
  import Skeleton from './Skeleton.vue';
</script>

<script>
  export default {
    data() {
      return {
        repoData: [],
        currentPage: 1,
        loading: false,
        perPage: 6,
        skeleton: [...new Array(6)],
        selectedLanguage: '', // Add selectedLanguage state to store the selected language filter
        searchTerm: '', // Add searchTerm state to store the search term
      };
    },
    methods: {
      fetchData() {
        this.loading = true;
        fetch(`https://api.github.com/users/Okezedavid/repos`, {
          headers: {
            Accept: "application/json"
          },
        })
        .then((res) => res.json())
        .then((data) => {
          this.repoData = data;
          this.loading = false;
        });
      },
      prevPage() {
        if (this.currentPage > 1) {
          this.currentPage--;
        }
      },
      nextPage() {
        if (this.currentPage < this.lastPage) {
          this.currentPage++;
        }
      },
      handleLanguageFilter(language) {
        this.selectedLanguage = language; // Update selectedLanguage state with the selected language
        this.currentPage = 1; // Reset currentPage to 1 when a new filter is applied
      },
    },
    mounted() {
      this.fetchData();
    },
    computed: {
      showMore() {
        const start = (this.currentPage - 1) * this.perPage;
        const end = start + this.perPage;
        this.loading = false;
        return this.repoData.filter(repo => {
          // Filter repos based on selectedLanguage if it's not empty
          if (this.selectedLanguage && repo.language !== this.selectedLanguage) {
            return false; // Skip if language doesn't match
          }
          // Filter repos based on searchTerm if it's not empty
          if (this.searchTerm && !repo.name.toLowerCase().includes(this.searchTerm.toLowerCase())) {
            return false; // Skip if name doesn't match
          }
          return true; // If no language selected or name matched, include the repo
        }).slice(start, end);
      },
      lastPage() {
        return Math.ceil(this.repoData.length / this.perPage);
      }
    }
  }
</script>

<template>
      <div className="welcomeMessage">
        <h2>
          Heyy!👋 <span className="welcome">Welcome</span>
        </h2>
      </div>
  <div>
    <!-- Your existing code -->
    <div class="main-inputs">
      <!-- Your input and select elements -->
      <input class="input" type="text" placeholder="Search repos here..." v-model="searchTerm">
      <select class="select-btn" v-model="selectedLanguage" @change="handleLanguageFilter(selectedLanguage)">
        <option value="">Filter</option>
        <option value="HTML">HTML</option>
        <option value="CSS">CSS</option>
        <option value="JavaScript">JavaScript</option>
        <!-- Add more options for other languages -->
      </select>

        </div>
        <div class="repoTitle">
        <h1>My Repositories</h1>
      </div>
        <div class="repo-container"> 
            <Skeleton v-if="loading" v-for="n in skeleton">{{ skeleton }}</Skeleton>  
            <div v-else v-for="repo in showMore" class="repo-card" :key="repo.id">
                <router-link :to="`/details/${repo.name}`"><h2 class="repo-name">{{ repo.name }}</h2></router-link>
                <p class="language">Langauge: {{ repo.language }}</p>
                <p class="date">Start date & time: {{ repo.created_at }}</p>
                <p class="visibility">Visibility: {{ repo.visibility }}</p>
            </div>
           
        </div>
        <div class="pagination">
            <button class="view-more" :class="currentPage === 1 ? 'disabled' : '' " @click="prevPage"><PrevIcon /></button>
            <p class="current-page">{{ currentPage }}</p>
            <button class="view-more" :class="currentPage === lastPage ? 'disabled' : '' " @click="nextPage"><NextIcon /></button>
        </div>
    </div>
  <footer class="foot">
    <div class="social-media">
        <!-- Twitter Icon and Link -->
        <a href="https://twitter.com/@DavidOkeze" target="_blank" class="social-link">
          <i class="fab fa-twitter"></i>
        </a>
        <!-- LinkedIn Icon and Link -->
        <a href="https://linkedin.com/in/david-ugochukwu-7172672b1" target="_blank" class="social-link">
          <i class="fab fa-linkedin"></i>
        </a>
      </div>

    <p>© 2024 David's Portfolio</p>
  </footer>
            
</template>


<style>


.welcomeMessage h2 {
  font-size: 2rem;
  font-style: italic;
  color: #dbe1a2;
  justify-content: center;
  text-align: center;
  margin-bottom: 50px;
  font-family: "Major Mono Display", monospace;
  overflow: hidden;
  white-space: nowrap;
  animation: typewriter 5s steps(40) forwards; /* Apply typewriter animation */
}

.welcome {
  color: #ddd6bc;
}

/* Animation for typewriter effect */
@keyframes typewriter {
  from {
    width: 0; /* Start with no width */
  }
  to {
    width: 100%; /* Increase width to full length */
  }
}


.input {
  width: 50%;
  font-size: 15px;
  height: 30px;
  padding: 17px;
  border: 1.3px solid #4a5779;
  border-radius: 10px;
  color: #8ba1d6;
  background-color: #1d1e22;
  font-weight: 750;
  align-items: center;
  justify-content: center;
  margin-left: 8%;
}

input:focus {
  outline: none; /* Remove the default outline */
  box-shadow: 0 0 5px 2px #9ddfe8; /* Blue shadow */
}

.select-btn {
  font-size: 15px;

  margin-top: 10px;

  height: 29px;
  padding: 5px;
  border: none;
  border-radius: 5px;
  color: #191c23;
  background-color: #1b9aaa;
  font-weight: 760;
  margin-left: 10px;
}

.select option {
  color: #f5f1e3;
}

.select-btn:hover {
  background-color: #36b4c5;
}


.repoTitle {
  font-size: 1.2rem;
  font-style: italic;
  color: #dcddce;
  justify-content: center;
  text-align: center;
  margin-bottom: 50px;
  font-family: "Major Mono Display", monospace;
  overflow: hidden;
  white-space: nowrap;
  padding-top: 3rem;
  /* animation: typewriter 3s steps(40), fadeOut 1s 3s forwards; Apply both animations */
}

.language,.date,.visibility {
    color: #8ba1d6;
  /* font-weight: 510; */
  font: 20px;
  font-family: "Major Mono Display", monospace;
}

.repo-container {
    display: grid;
    grid-template-columns: repeat(auto-fit,minmax(300px,1fr));
    grid-gap: 30px;
    width: 90%;
    margin: 0 auto;
    margin-bottom: 5rem;
}


@keyframes fadeIn {
  from {
    opacity: 0;
    transform: translateY(20px);
  }
  to {
    opacity: 1;
    transform: translateY(0);
  }
}

.repo-card {
  animation: fadeIn 1.5s ease-out forwards;
  transition: transform 0.3s ease-in-out, box-shadow 0.3s ease-in-out;

}

.repo-card,
.repodetail-card {
  border: 1px solid #1b9aaa;
  padding: 15px;
  border-radius: 15px;
  box-shadow: #7c8db5;
}



.repo-card:hover {
  transform: rotate(5deg);

  box-shadow: 0 0 10px #9ddfe8;
}

a {
  text-decoration: none;
  color: #0d7591;
}

a:hover {
  text-decoration: underline;
}

.repo-name {
  color: #0e5f76;
  font-size: 2rem;
  word-break: break-word;
}

p {
    padding: 5px 0;
}

.pagination {
    display: flex;
    align-items: center;
    justify-content: center;
    gap: 1rem;
    margin: 3rem 0;
}

button {
    background: none;
    border: none;
    cursor: pointer;
}

.view-more svg {
    width: 2rem;
}

.disabled {
    cursor: not-allowed;
    opacity: 0.5;
}

.foot {
  color: #f5f1e3;
  text-align: center;
  padding: 20px;
  position: relative;
  bottom: 10px;
  width: 100%;
  font-size: 15px;
}

@media screen and (max-width: 480px) {
  .repo-container {
    grid-template-columns: repeat(auto-fit, minmax(200px, 1fr));
  }

  .repo-name {
    font-size: 20px;
  }
  

  h1 {
    font-size: 1.5rem;
  }

  .github-icon {
    font-size: 2rem;
  }
  /* .input {
    width: 60%;
    margin-left: 20px;
  } */
}

</style>