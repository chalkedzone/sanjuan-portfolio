<template>
    <section id="projects" class="container-fluid py-5 px-5">
        <h2 class="text-center">PROJECTS</h2>

        <!-- Loop through each group of 3 projects -->
        <div
            v-for="(group, index) in chunkedProjects"
            :key="index"
            class="row mt-5 mb-4"
        >
            <!-- For each project in the current group, render a ProjectCard -->
            <div
                v-for="project in group"
                :key="project.id"
                class="col-md-4 col-sm-12 mb-4 d-flex justify-content-center"
            >
                <ProjectCard :project="project" />
            </div>
        </div>
    </section>
</template>

<script setup>
import { computed } from 'vue';
import ProjectCard from './ProjectCard.vue';
import projects from '../data/projects.json';

const chunkSize = 3;

const chunkedProjects = computed(() => {
    const chunks = [];
    for (let i = 0; i < projects.length; i += chunkSize) {
        chunks.push(projects.slice(i, i + chunkSize));
    }
    return chunks;
});

</script>
