package com.testingAPI.blog.services.impl;

import java.io.File;
import java.io.FileInputStream;
import java.io.FileNotFoundException;
import java.io.IOException;
import java.io.InputStream;
import java.nio.file.Files;
import java.nio.file.Paths;
import java.util.UUID;

import org.modelmapper.ModelMapper;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Service;
import org.springframework.web.multipart.MultipartFile;

import com.testingAPI.blog.entity.Post;
import com.testingAPI.blog.payload.PostDto;
import com.testingAPI.blog.repository.PostRepo;
import com.testingAPI.blog.repository.UserRepo;
import com.testingAPI.blog.services.FileService;
import com.testingAPI.blog.services.PostService;

@Service
public class FileServiceImpl implements FileService {

	@Autowired
	private UserRepo userRepo;
	
	@Autowired
	private PostRepo postRepo;
	@Autowired
	private PostService postService;
	@Autowired
	private ModelMapper modelMapper;
	
	@Override
	public String uploadImage(String path, MultipartFile file, Integer postID) throws IOException {
		//file name 
		String name=file.getOriginalFilename();
		
		//random filename
		
		String randomID = UUID.randomUUID().toString();
		String fileName1 = randomID.concat(name.substring(name.lastIndexOf(".")));
		
		PostDto post = postService.getSinglePost(postID);
		post.setImageName(fileName1);
		this.postService.updatePost(post, postID);
		//postRepo.save(modelMapper.map(post, Post.class));
		
		//FullPath
		String filePath=path+File.separator+fileName1;
		
		//create folder if not created
		File f=new File(path);
		if(!f.exists()) { f.mkdir(); }
		
		//file copy to fullPatth
		Files.copy(file.getInputStream(), Paths.get(filePath));
		
		return name;
	}

	@Override
	public InputStream getResource(String path, String filename) throws FileNotFoundException {
		String fullPath = path+File.separator+filename;
		InputStream is = new FileInputStream(fullPath);
		
		
		return is;
	}

}
