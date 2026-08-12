<style>
    .social-icons {
        margin-top: 1rem;
        display: flex;
        gap: 1rem;
    }

    .social-icons a {
        font-size: 2rem;
        color: #021D9E;
    }

    .social-icons a:hover {
        color: #0d6efd;
    }
    
</style>

<template>
    <section id="contact" class="container-fluid py-5 px-5">
        <h2 class="text-center">Contact Me</h2>
        <div class="row">
            <div class="col-md-6">
                <form @submit.prevent="submitForm">
                    <div class="form-group">
                        <label>
                            Name
                            <input type="text" v-model="name" name="name" class="form-control" placeholder="Enter your name" required>
                        </label>
                    </div>
                    <div class="form-group">
                        <label>
                            Email
                            <input type="email" v-model="email" name="email" class="form-control" placeholder="Enter your email" required>
                        </label>
                    </div>
                    <div class="form-group">
                        <label>
                            Message
                            <textarea v-model="message" name="message" class="form-control" rows="5" placeholder="Enter your message" required></textarea>
                        </label>
                    </div>
                    <div class="form-footer d-flex justify-content-between align-items-center mt-3">
                        <button type="submit" class="btn" :disabled="isLoading">
                        {{ isLoading ? 'Sending...' : 'Submit' }}
                        </button>
                        <div class="social-icons">
                            <a href="https://www.facebook.com/enzo.stjohn" id="facebook"><i class="fab fa-facebook"></i></a>
                            <a href="https://www.linkedin.com/in/enrico-lorenzo-san-juan-0384b92b2/" id="linkedin"><i class="fab fa-linkedin"></i></a>
                            <a href="https://github.com/chalkedzone" id="github"><i class="fab fa-github"></i></a>
                        </div>
                    </div>
                    
                </form>
            </div>
            <div class="col-md-6 d-flex flex-column align-items-center">
                <h3 class="p-1">Map</h3>
                <iframe title="Map showing my location" src="https://www.google.com/maps/embed?pb=!1m18!1m12!1m3!1d15447.000198053964!2d120.9991027!3d14.556281100000001!2m3!1f0!2f0!3f0!3m2!1i1024!2i768!4f13.1!3m3!1m2!1s0x3397c97b2307534f%3A0x8e93d2f6ba85fbd!2sAvida%20Towers%20Prime%20Taft%2C%20Tower%202!5e0!3m2!1sen!2sph!4v1779275438620!5m2!1sen!2sph" loading="lazy" referrerpolicy="no-referrer-when-downgrade" class="rounded"></iframe>
            </div>
        </div>
    </section>
</template>

<script setup>
    import { ref } from 'vue'
    import { Notyf } from 'notyf'
    import 'notyf/notyf.min.css'

    const notyf = new Notyf()
    const WEB3FORMS_ACCESS_KEY = "051dd95b-74be-4474-a34c-45673020f8e0"
    const subject = "New message from Portfolio Contact Form"

    const name = ref('')
    const email = ref('')
    const message = ref('')
    const isLoading = ref(false)

    const submitForm = async () => {
        isLoading.value = true
        try {
            const response = await fetch('https://api.web3forms.com/submit', {
                method: "POST",
                headers: {
                    "Content-Type": "application/json",
                    Accept: "application/json"
                },
                body: JSON.stringify({
                    access_key: WEB3FORMS_ACCESS_KEY,
                    subject: subject,
                    name: name.value,
                    email: email.value,
                    message: message.value
                })
            })
            const result = await response.json()

            if(result.success) {
                console.log(result)
                isLoading.value = false
                notyf.success("Message Sent!")
            }
        } catch(error){
            console.log(error)
            isLoading.value = false
            notyf.error("Failed to send message!")
        }
    }
</script>