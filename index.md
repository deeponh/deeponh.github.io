---
layout: default
---

## About Me
<tr>
    <td><img class="profile-picture" src="me_deepon.png"></td>
    <td><div class="profile-doc">
		Intern @ AI4Bharat, IIT Madras <br>
        UG @ IIEST Shibpur <br>
        <br>
		<a href="mailto:deeponh.2004@gmail.com">
			<i class="fa fa-envelope" aria-hidden="true"></i> deeponh.2004@gmail.com</a> <br> 
		<a href="https://github.com/cneuralnetwork">
			<i class="fa fa-github" aria-hidden="true"></i> Github </a> <br> 
		<a href="#">
			<i class="fa fa-google" aria-hidden="true"></i> Google Scholar </a> <br> 
		<a href="https://www.linkedin.com/in/deeponhalder/">
			<i class="fa fa-linkedin" aria-hidden="true"></i> LinkedIn </a>
        <br><br>
        <!-- VAT ID: FI-3485521-8 -->
        <br>
	</div></td>
</tr>

I am from India 🇮🇳. I work as a Research Intern at AI4Bharat, IIT Madras under the guidance of <a href="https://prajdabre.github.io/">Dr. Raj Dabre</a>. I am also known as <a href="https://cneuralnets.netlify.app">neuralnets</a>. Feel free to contact me for any queries.








## Publications
{% assign years = site.data.papers | group_by:"year" | sort: "name" | reverse %}

{% for year in years %}
### {{ year.name }}	
---

{% for paper in year.items %}
<table class="paper-list">
  <tr>
  	{% if paper.paper-logo %}
    <td><img class="paper-logo" src="{{paper.paper-logo}}"></td>
	{% endif %}
	{% if paper.paper-logo-mp4 %}
    <td>
		<div class="paper-logo">
		<video width="100%" height="100%" muted autoplay loop>
			<source src="{{paper.paper-logo-mp4}}" type="video/mp4">
			Your browser does not support the video tag.
		</video>
		</div>
	</td>
	{% endif %}
    <td>
		<p class="paper-title">{{paper.paper-title}}</p>  
		<p class="paper-authors">
			{% for author in paper.paper-authors %}
				{% if forloop.last == true %}
					{{author.name}}.
				{% else %}
					{{author.name}},
				{% endif %}
			{% endfor %}
		</p>
		<p class="paper-pub">{{paper.paper-pub}}</p>
		<p class="paper-links">
			{% if paper.link-pdf %}
			<a href="{{paper.link-pdf}}" target="_blank" rel="noopener">
				<i class="fa fa-file-pdf-o" aria-hidden="true"></i> PDF </a>
			{% endif %}

			{% if paper.link-arxiv %}
			<a href="{{paper.link-arxiv}}" target="_blank" rel="noopener">
				<i class="fa fa-file" aria-hidden="true"></i> arXiv </a> 
			{% endif %}

			{% if paper.link-projectpage %}
			<a href="{{paper.link-projectpage}}" target="_blank" rel="noopener">
				<i class="fa fa-link" aria-hidden="true"></i> ProjectPage </a>  
			{% endif %}

			{% if paper.link-supplementary %}
			<a href="{{paper.link-supplementary}}" target="_blank" rel="noopener">
				<i class="fa fa-file-pdf-o" aria-hidden="true"></i> Supplementary </a>  
			{% endif %}

			{% if paper.link-gitcode %}
			<a href="{{paper.link-gitcode}}" target="_blank" rel="noopener">
				<i class="fa fa-github" aria-hidden="true"></i> Github </a>  
	        {% endif %}

			{% if paper.link-gitcode-button %}
			<a href="{{paper.link-gitcode}}" target="_blank" rel="noopener">
				<i class="fa " aria-hidden="true"></i></a>  
				<iframe style="margin-left: 2px; margin-bottom:-5px;" 
					frameborder="0" scrolling="0" width="100px" height="20px"
	                src="{{paper.link-gitcode-button}}">
	        	</iframe>
	        {% endif %}

	        {% if paper.link-gitdata %}
			<a href="{{paper.link-gitdata}}" target="_blank" rel="noopener">
				<i class="fa fa-github" aria-hidden="true"></i> Dataset </a> 
	        {% endif %}

	        {% if paper.link-video %}
	        <a href="{{paper.link-video}}" target="_blank" rel="noopener">
				<i class="fa fa-file-video-o" aria-hidden="true"></i> Video </a> 
			{% endif %}

			{% if paper.link-video1 %}
	        <a href="{{paper.link-video1}}" target="_blank" rel="noopener">
				<i class="fa fa-file-video-o" aria-hidden="true"></i> Video1 </a> 
			{% endif %}

			{% if paper.link-video2 %}
	        <a href="{{paper.link-video2}}" target="_blank" rel="noopener">
				<i class="fa fa-file-video-o" aria-hidden="true"></i> Video2 </a> 
			{% endif %}

			{% if paper.link-slides %}
	        <a href="{{paper.link-slides}}" target="_blank" rel="noopener">
				<i class="fa fa-files-o" aria-hidden="true"></i> Slides </a> 
			{% endif %}

			{% if paper.link-poster %}
	        <a href="{{paper.link-poster}}" target="_blank" rel="noopener">
				<i class="fa fa-file" aria-hidden="true"></i> Poster </a> 
			{% endif %}

		</p>
	</td>
  </tr>
</table>
{% endfor %}
{% endfor %}

---
## Projects
<tr>
    <td><div>
        <a href="https://github.com/cneuralnetwork/solving-ml-papers">
			<i class="fa fa-github" aria-hidden="true"></i> paper to code :</a> implementation of research papers into code
            <br> 
            <br> 
        <a href="https://github.com/neu-reseau/neugrad">
			<i class="fa fa-github" aria-hidden="true"></i>  neugrad :</a> Built a lightweight autograd engine in Python and NumPy, with tensor operations, automatic differentiation, backpropagation, and a scalable design for deep learning, mimicking PyTorch for education and experimentation.
            <br> 
	</div></td>
</tr>
---
## Technical Blogs
<tr>
   <td><div>
       <a href="https://trite-song-d6a.notion.site/Linear-Regression-SS-1-11c0af77bef380bab612fbc77e447caf">
           linear regression:</a> A deep dive into Linear Regression and the math behind it.
           <br>
           <br>
       <a href="https://trite-song-d6a.notion.site/Transformers-Week-2-1210af77bef38088b253e28a351b7212">
           transformers:</a> a deep dive into transformers and a visual guide into how it works.
           <br>
           <br>
       <a href="https://trite-song-d6a.notion.site/Optimizers-Week-3-12a0af77bef380d6be27f0a7db7dc533">
           optimizers:</a> a deep dive into optimizers and a journey through time.
           <br>
           <br>
       <a href="https://trite-song-d6a.notion.site/RNNs-Week-4-12e0af77bef38040a8d9f10cff7bf295">
           rnn:</a> a deep dive into recurrent neural networks and how the math behind it works.
           <br>
           <br>
       <a href="https://trite-song-d6a.notion.site/Basics-of-NLP-1-Week-5-1340af77bef380718a51e0e9ee84a4bd">
           basics of nlp [part 1]:</a> discussion into how text preprocessing, regex, frequencies, and word embeddings work.
           <br>
           <br>
       <a href="https://trite-song-d6a.notion.site/Basics-of-NLP-2-Week-6-14b0af77bef3808d8aabdb1e744b888e">
           basics of nlp [part 2]:</a> discussion into how pos tagging, ner, sentiment analysis, and n-gram models work.
           <br>
           <br>
       <a href="https://trite-song-d6a.notion.site/Basics-of-NLP-3-Week-7-1590af77bef38029a0d5f08dafc86594">
           basics of nlp [part 3]:</a> a guide into how hidden markov models, text clustering, attention work.
           <br>
           <br>
       <a href="https://trite-song-d6a.notion.site/LLMs-1-Week-8-1660af77bef380039facddb7502c2ef1">
           llms [part 1]:</a> a guide into how embeddings, positional embeddings, tokenizer (especially bpe tokenizer) work.
           <br>
           <br>
       <a href="https://trite-song-d6a.notion.site/Enhance-Your-Model-1-Week-9-17d0af77bef38077a451feb454ae5152">
           enhance your model [part 1]:</a> a guide into how lora, model distillation, gradient clipping and early stopping work.
           <br>
           <br>
       <a href="https://trite-song-d6a.notion.site/LLMs-2-Week-10-18f0af77bef3808fb543c0a39155b0cf">
           llms [part 2]:</a> a guide into how attention works, in great detail.
           <br>
           <br>
       <a href="https://trite-song-d6a.notion.site/Deepseek-R1-for-Everyone-1860af77bef3806c9db5e5c2a256577d">
           deepseek r1 explanation:</a> a guide into how deepseek r1 works under the hood.
           <br>
           <br>
       <a href="https://medium.com/@cneuralnetworks/all-about-quantization-5b258f60ab83">
           all about quantization:</a> a guide into how quantization occurs in llms.
           <br>
   </div></td>
</tr>
---
## Past Experience
<tr>
    <td><div>
        <a href="https://drive.google.com/file/d/1Zd9U8XNWYN_atgvO0wz4qdHgxpQoFLbo/view?usp=sharing">
			Research Intern :</a> IIT Bhubaneshwar [Dec 2024 - Jan 2025] 
            <br> 
            <br> 
			<a href="#">
			Research Apprentice :</a> IIEST Shibpur [July 2024 - Sept 2024]
            <br> 
            <br> 
        
	</div></td>
</tr>
---
## Invited talks

Date | Event | Details
-----|-------|--------
May, 21st 2025 | SRM IST Campus| kk
---
<br>
> If you can’t explain something to a first-year student, then you haven’t really understood it. - R. Feynman