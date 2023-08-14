package com.testingAPI.blog.services.impl;

import java.util.Date;
import java.util.List;
import java.util.stream.Collectors;

import org.modelmapper.ModelMapper;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.data.domain.Page;
import org.springframework.data.domain.PageRequest;
import org.springframework.data.domain.Pageable;
import org.springframework.data.domain.Sort;
import org.springframework.stereotype.Service;

import com.testingAPI.blog.entity.Category;
import com.testingAPI.blog.entity.Post;
import com.testingAPI.blog.entity.User;
import com.testingAPI.blog.exception.ResourceNotFoundException;
import com.testingAPI.blog.payload.PostDto;
import com.testingAPI.blog.payload.PostResponse;
import com.testingAPI.blog.repository.CategoryRepo;
import com.testingAPI.blog.repository.PostRepo;
import com.testingAPI.blog.repository.UserRepo;
import com.testingAPI.blog.services.PostService;

@Service
public class PostServiceImpl implements PostService {

	@Autowired
	private PostRepo postRepo;
	
	@Autowired
	private ModelMapper modelMapper;
	@Autowired
	private UserRepo userRepo;
	@Autowired
	private CategoryRepo categoryRepo;
	
	@Override
	public PostDto createPost(PostDto postDto,Integer userID,Integer categoryID){
		User user = this.userRepo.findById(userID).orElseThrow(()-> new ResourceNotFoundException("User", "ID", userID));
		Category cat = this.categoryRepo.findById(categoryID).orElseThrow(()-> new ResourceNotFoundException("Category", "ID", categoryID));
		Post post = this.modelMapper.map(postDto, Post.class);
		post.setImageName("default.png");
		post.setAddeddate(new Date());
		post.setUser(user);
		post.setCategory(cat);
		Post createdPost= this.postRepo.save(post);
		return this.modelMapper.map(createdPost, PostDto.class);
	}

	@Override
	public PostDto updatePost(PostDto postDto, Integer postID) {
		Post oldPost = this.postRepo.findById(postID).orElseThrow(()-> new ResourceNotFoundException("post", "ID", postID));
		oldPost.setTitle(postDto.getTitle());
		oldPost.setContent(postDto.getContent());
		oldPost.setImageName(postDto.getImageName());
		Post updatedPost = this.postRepo.save(oldPost);
		return this.modelMapper.map(updatedPost, PostDto.class);
	}

	@Override
	public void deletePost(Integer postID) {
		Post post = this.postRepo.findById(postID).orElseThrow(()-> new ResourceNotFoundException("post", "ID", postID));
		this.postRepo.delete(post);
	}

	@Override
	public PostResponse getAllPost(Integer pageNum, Integer PageSize,String sortBy, String sortDir) {
		Sort sort=null;
		if(sortDir.equalsIgnoreCase("asc")) 
		{
			sort = Sort.by(sortBy).ascending();
		}
		else {
			sort = Sort.by(sortBy).descending();
		}
		
		Pageable p = PageRequest.of(pageNum, PageSize, sort);
		Page<Post> pagePost = this.postRepo.findAll(p);
		List<Post> allPost = pagePost.getContent();
		List<PostDto> postDtos = allPost.stream().map(x -> this.modelMapper.map(x, PostDto.class)).collect(Collectors.toList());
		
		PostResponse postResponse = new PostResponse();
		postResponse.setContent(postDtos);
		postResponse.setPageNum(pagePost.getNumber());
		postResponse.setPageSize(pagePost.getSize());
		postResponse.setTotalElements(pagePost.getTotalElements());
		postResponse.setTotalPages(pagePost.getTotalPages());
		postResponse.setLastPage(pagePost.isLast());
		
		return postResponse;
	}

	@Override
	public PostDto getSinglePost(Integer postID) {
		Post singlePost = this.postRepo.findById(postID).orElseThrow(()-> new ResourceNotFoundException("post", "ID", postID));
		return this.modelMapper.map(singlePost, PostDto.class);
	}

	@Override
	public List<PostDto> getPostByCategory(Integer categoryId) {
		Category cat= this.categoryRepo.findById(categoryId).orElseThrow(()-> new ResourceNotFoundException("Category", "ID", categoryId));
		List<Post> postbyCat= this.postRepo.findByCategory(cat);
		return postbyCat.stream().map(p -> this.modelMapper.map(p, PostDto.class)).collect(Collectors.toList());
		
	}

	@Override
	public List<PostDto> getPostByUser(Integer userID) {
		User user = this.userRepo.findById(userID).orElseThrow(()-> new ResourceNotFoundException("User", "ID",userID));
		List<Post> postbyUser = this.postRepo.findByUser(user);
		return postbyUser.stream().map(p -> this.modelMapper.map(p, PostDto.class)).collect(Collectors.toList());
	}

	@Override
	public List<PostDto> searchPost(String keyword) {
		List<Post> posts = this.postRepo.findByTitleContaining(keyword);
		return posts.stream().map(p-> this.modelMapper.map(p, PostDto.class)).collect(Collectors.toList());
	}

}
